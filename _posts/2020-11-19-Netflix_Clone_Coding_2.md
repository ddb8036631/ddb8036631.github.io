---
title: "Netflix Clone Coding(2/4)"
date: 2020-11-19 00:38:40 +0900
categories:
  - Clone Coding
toc: true
toc_sticky: true
---

# 모든 영화 가져오기

## requests.js 파일 생성

src 폴더 밑에 request.js 파일을 하나 생성합니다. 컴포넌트로 사용될 목적이 아닌 함수들을 모아둘 목적으로 생성되는 파일이므로 소문자로 시작합니다.

요청 목록은 아래와 같습니다. 각각의 key(요청 이름)에 해당하는 value(URL)들은 TMDB의 docs를 참고해서 작성한 것으로 보입니다.

```jsx
// requests.js

const API_KEY = "f96ef394de8f7c28775817aeefc3cadd";

const requests = {
  fetchTrending: `/trending/all/week?api_key=${API_KEY}&language=en-US`,
  fetchNetflixOriginals: `/discover/tv?api_key=${API_KEY}&with_networks=213`,
  fetchTopRated: `/movie/top_rated?api_key=${API_KEY}&language=en-US`,
  fetchActionMovies: `/discover/movie?api_key=${API_KEY}&with_genres=28`,
  fetchComedyMovies: `/discover/movie?api_key=${API_KEY}&with_genres=35`,
  fetchHorrorMovies: `/discover/movie?api_key=${API_KEY}&with_genres=27`,
  fetchRomanceMovies: `/discover/movie?api_key=${API_KEY}&with_genres=10749`,
  fetchDocumentaries: `/discover/movie?api_key=${API_KEY}&with_genres=99`,
};

export default requests;
```

## axios.js 파일 생성

위의 requests 객체 각각에 해당하는 axios 요청을 미리 작성해두는 파일입니다.

기본 포맷은 아래와 같습니다. baseURL을 설정해 둔 axios 요청 객체에 덧붙여 코드를 작성해서 요청을 보낼 예정입니다.

```jsx
import axios from "axios";

const instance = axios.create({
  baseURL: "https://api.themoviedb.org/3",
});

export default instance;
```

## Row.js 파일 생성

![](/assets/images/netflix.png)

위의 사진에서의 rows(한 줄 한 줄)은 각각 하나의 컴포넌트입니다.

React에서는 컴포넌트를 재사용해서 각기 다른 props 를 적용해 화면을 구성하게 됩니다.

이에 따라 App.js에 다음과 같은 코드를 작성합니다.

```jsx
// App.js

function App() {
  return (
    <div className="App">
      <Row title="NETFLIX ORIGINALS" />
      <Row title="Trending Now" />
      <Row title="Top Rated" />
      <Row title="Action Movies" />
      <Row title="Comedy Movies" />
    </div>
  );
}

export default App;
```

그리고 Row.js라는 컴포넌트 구성하는 파일을 생성한 뒤 아래와 같은 코드를 작성합니다.

```jsx
import React, { useState, useEffect } from "react";

const Row = ({ title, fetchUrl }) => {
  const [movies, setMovies] = useState([]);

  useEffect(() => {}, []);

  return (
    <div>
      <h2>{title}</h2>

      {/* container -> posters  */}
    </div>
  );
};

export default Row;
```

useEffect(() ⇒ {}, []); 는 컴포넌트가 최초로 mount 될 때 한 번만 실행되는 함수입니다.

이 때 TMDB에 요청을 보내는 과정을 거칩니다.

async await를 통해 보낸 요청이 돌아올 때 까지 기다렸다가 응답이 오면 setMovies 함수를 통해 state에 저장합니다.

useEffect Hook 안에서 props 를 사용한다면, 해당 props에 의존하기 때문에 useEffect의 deps 배열에 작성해줘야 합니다. 해당 props 가 변한다면, useEffect를 통해 데이터를 업데이트해줘야 하기 때문입니다.

fetchUrl을 다르게 여러번 컴포넌트를 호출할 건데, deps 배열을 빈 배열로 둔다면 처음 컴포넌트가 생성될 한 번만 호출될 것으로 보임.

배열과 객체를 console 에 찍을 때 console.table() 도 유용하게 쓰일 수 있습니다.

map 함수를 돌며 return 하는 img 태그에 key 속성을 줌으로써 더 빠른 렌더링이 가능하도록 최적화 할 수 있습니다.

## poster_path와 backdrop_path 다르게 설정하기

NETFLIX ORIGINALS row만 포스터의 크기가 크고 비율이 다른 것을 확인할 수 있습니다.

포스터의 비율과 크기가 다른 NETFLIX ORIGINALS row 컴포넌트에 isLargeRow 라는 property를 주고(App.js), className을 다르게 설정할 수 있습니다(Row.js).

prop 명만 주면, true 값을 보내는 것과 같이 작동합니다. 따라서, isLargeRow 만 적는 것과 isLargeRow={true}는 같은 의미를 가집니다.

```jsx
// App.js

<Row
  title="NETFLIX ORIGINALS"
  fetchUrl={requests.fetchNetflixOriginals}
  isLargeRow
/>
<Row title="Trending Now" fetchUrl={requests.fetchTrending} />
```

```jsx
// Row.js

<img
  key={movie.id}
  className={`row__poster ${isLargeRow && "row__posterLarge"}`}
  src={`${base_url}${isLargeRow ? movie.poster_path : movie.backdrop_path}`}
  alt={movie.name}
/>
```

## Row.css 파일 생성

row**posters 클래스에 display: flex 를 줌으로써 각각의 row**poster 들이 가로 방향으로 정렬되도록 합니다.

max-height를 지정함으로써 포스터 사진의 가로, 세로 비율을 유지한 채 크기를 바꿉니다.

사진을 hover했을 때 그 크기가 1.08배로 커지면서, transition 속성에 transform할 때 450ms 의 시간 duration을 줌으로써 천천히 바뀌게 됩니다.

```jsx
.row__posters::-webkit-scrollbar {
  // 모든 브라우저(firefox, chrome, edge ...)에 스크롤바를 없애도록 적용.
  display: none;

}
```
