---
title: "[블로그 관련] Ruby, Jekyll 설치"
date: 2020-11-22 20:42:43 +0900
categories:
  - blog
toc: true
classes: wide
---

맥에는 2.6.3 버전의 시스템 ruby가 설치되어 있습니다.

jekyll 홈페이지에서 jekyll은 ruby 2.4.0 버전 이상을 필요로 한다 하지만, 시스템 루비를 사용해 jekyll을 설치하려고 하면 FilePermissionError로 인해 제대로 작동하지 않는 모습을 볼 수 있습니다.

따라서 ruby의 버전을 관리해주는 rbenv를 통해 설치한 뒤 해당 버전의 환경에서 jekyll 및 bundler를 설치합니다.

아래 명령어를 순차적으로 실행해 rbenv와 2.6.5 버전의 ruby를 설치합니다.

```bash
brew install rbenv ruby-build
rbenv install 2.6.5
rbenv global 2.6.5
```

아래 명령어로 rbenv 환경에 설치된 루비 버전을 확인합니다.

```bash
rbenv versions
```

zsh 셸 설정파일에 아래 두 줄의 명령어를 입력해 환경 변수를 설정합니다.

```bash
export PATH={$Home}/.rbenv/bin:$PATH && \
eval "$(rbenv init -)"
```

아래 명령어를 통해 변경된 설정을 적용합니다.

```bash
source ~/.zshrc
```

이후 gem 명령어를 통해 jekyll 및 bundler를 설치합니다.

```bash
gem install jekyll bundler
```

아래 명령어를 통해 로컬에서 jekyll 프로젝트를 실행합니다.

```bash
bundle install
bundle exec jekyll serve
```