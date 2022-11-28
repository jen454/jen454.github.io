# jen454.github.io

## **우분투 리눅스 환경에서 깃블로그 만들기**

[![screenshot](/images/homescreen.png)](https://jen454.github.io/)
**!사진을 클릭하면 블로그로 넘어갑니다!**

## Build 과정

### 1. Repository 생성
- Github에서 <username>.github.io 이름의 Repository 생성

### 2. Local-Remote Repository 연동
1. Remote Repository의 주소를 복사
2. `git clone <복사한 Remote repository의 주소> <path>`로 clone
3. `git commit -m "<commit msg>"`로 커밋 남기기
4. `git branch -M main`으로 현재 branch의 이름을 main으로 변경
5. `git status`로 현재 상태 확인 후 `git add .`로 변경파일 추가
6. `git push origin main`으로 main에 로컬 변경사항 push
    - push를 위한 개인 token 생성 필수!

### 3. Jekyll 설치

- [Ubuntu linux용 jekyll 가이드 참조](https://jekyllrb-ko.github.io/docs/installation/ubuntu/)
  - 위 링크를 이용해서 Jekyll을 설치하기 전, 필요한 모든 의존요소를 가지고 있는지 확인하고 일반 사용자 계정에 젬 설치 디렉토리를 설정
- Jekyll과 Bundler 설치
  - `gem install jekyll bundler`
- Jekyll이 올바르게 설치되었는지 확인
  - `jekyll -v`

### 4. Jekyll 사이트 생성

- 현재 디렉토리(.)(=clone한 디렉토리)에 Jekyll을 설치
  - `jekyll new . --force`
- Jekyll 시작
  - `bundle exec jekyll serve`
  - localhost:4000 접속
- **LoadError 발생 시 webrick 파일 설치**
  - `bundle add webrick`

### 5. 테마 적용하기

- [다음](http://jekyllthemes.org/)과 [다음](https://jekyllthemes.io/free)에서 원하는 테마 선택
- 선택한 테마를 fork
- fork 후, username.github.io 이름의 Repository 생성
- 2번부터 차례대로 진행

### 6. 사용자화

- 블로그 이름, 프로필 사진 등 **_config.yml**에서 수정

  ```
  # Site settings
  title: "Jinwook's blog"
  description: "What's important is an unbreakable heart."
  baseurl: "" # the subpath of your site, e.g. "/blog"
  url: # the base hostname & protocol for your site e.g. "https://mmistakes.github.io"
  logo: "/images/Jinwook_profile.jpg" # path of site logo, e.g. "/assets/images/logo.png"
  ```

- 블로그 포스팅은 **_posts** 폴더에서 진행
  - _post에 **YYYY-MM-DD-TITLE.md** 형태로 새로운 문서를 작성

  ```
  ---
  layout: post
  title: "제목"
  categories:
    - "카테고리 이름"
  tags:
    - "태그 이름"
  comments: true
  last_modified_at: 2022-11-26T13:56:10-58:00
  ---
  ```

  - 위와 같은 형식으로 post 문서 작성
  - Markdown 형식을 통해 작성

**Google-Analytics 기능 추가 포스팅은 블로그 내에 있음**

- github, instagram 계정 연동 **_config.yml**에서 수정

  ```
  # Site Author
  author:
    name: Shin jin wook
    picture: /images/Jinwook_profile.jpg
    email: jinwook2765@naver.com
    links:
      - title: Instagram
        url: https://instagram.com/j_wxxk__/
        icon: fab fa-instagram
      - title: GitHub
        url: https://github.com/jen454/
        icon: fab fa-github-square
  ```
  ```
  # Footer Links
  footer_links:
    - title: Instagram
      url: https://instagram.com/j_wxxk__/
      icon: fab fa-instagram
    - title: GitHub
      url: https://github.com/jen454/
      icon: fab fa-github-square
  ```

- home화면 profiletext 수정
  - **_layouts**에서 home.html 수정
  ```
  ---
  layout: page
  ---

  {{ content }}

  <div class="entries-{{ page.entries_layout | default: 'list' }}">
  </div>
  ```
  - **index.me**파일 추가 후 내용 수정
  ```
  ---
  layout: home
  ---

  ## About Me

  - 2002/04/14
  - South-Korea/Seoul/Guro

  ## Career

  - Kookmin Univ. Software (2022/03 ~ )

  ## Interests

  - composing and writing lyrics for the hip-hop genre based on black music such as rap and R&B.
  - Baekjun problem solving in Java and Python languages.
  ```

### 7. 댓글 기능 추가

  1. [Disqus](https://disqus.com/) 가입
  2. "I want to install Disqus on my site" 선택
  3. 사이트 정보 입력 (이때, **Website Name** 기억하기)
  4. Platform 중 **Jekyll** 선택
  5. Install Instruction을 읽어본 후 **Configure**를 눌러 다음을 진행
  6. **Website URL**에 자신 블로그 주소 입력 후 Next 이동
  7. Comment 정책 선택
  8. Complete Setup을 눌러 설정 마무리
  9. **_config.yml**에 다음과 같은 key-value 추가

  ```
  comment:
    provider: "disqus"
    disqus:
      shortname1: "git"
      shortname2: "github"
      shortname3: "markdown"
      shortname4: "google-analytics"
  ```

  10. **_layout/post.html**에 Universal Code 적용 (PAGE_URL과 PAGE_IDENTIFIER은 나의 정보에 맞게 수정)

  ```
  {% if page.comments %}
  <h2>Comments</h2>
  <div id="disqus_thread"></div>
  <script>
      /**
      *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
      *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    */
      let PAGE_URL = "{{site.url}}{{page.url}}";
      let PAGE_IDENTIFIER = "{{page.url}}";
      var disqus_config = function () {
      this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
      this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
      };
      (function() { // DON'T EDIT BELOW THIS LINE
      var d = document, s = d.createElement('script');
      s.src = 'https://jen454.disqus.com/embed.js';
      s.setAttribute('data-timestamp', +new Date());
      (d.head || d.body).appendChild(s);
      })();
  </script>
  <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
  {% endif %}
  ```
  11. 내가 만든 각 post 파일들에 아래 코드 추가

  ```
  comments: true
  ```

### 8. 파비콘(Favicon) 추가

- [여기](https://realfavicongenerator.net/)에 접속해서 원하는 사진 넣기
- 다운로드를 받고 압축 폴더를 받기
- 현재 디렉토리의 assets 폴더 내의 logo.ico라는 폴더를 만들고 여기에 압축 풀기
- 다운로드 하고 난 사이트 가장 하단에서 **Generate your Favicons and HTML code** 버튼 누르기
- 생성된 코드를 **_includes/head-custom.html**에 적기

  ```
  <!-- start custom head snippets -->

  <!-- insert favicons. use http://realfavicongenerator.net/ -->
  <link rel="apple-touch-icon" sizes="180x180" href="{{site.baseurl}}/assets/logo.ico/apple-touch-icon.png">
  <link rel="icon" type="image/png" sizes="32x32" href="{{site.baseurl}}/assets/logo.ico/favicon-32x32.png">
  <link rel="icon" type="image/png" sizes="16x16" href="{{site.baseurl}}/assets/logo.ico/favicon-16x16.png">
  <link rel="manifest" href="{{site.baseurl}}/assets/logo.ico/site.webmanifest">
  <link rel="mask-icon" href="{{site.baseurl}}/assets/logo.ico/safari-pinned-tab.svg" color="#5bbad5">
  <link rel="shortcut icon" href="{{site.baseurl}}/assets/logo.ico/favicon.ico">
  <meta name="msapplication-TileColor" content="#da532c">
  <meta name="msapplication-config" content="{{site.baseurl}}/assets/logo.ico/browserconfig.xml">
  <meta name="theme-color" content="#ffffff">

  <!-- end custom head snippets -->
  ```
  - 파일 이름 앞에 **{{site.baseurl}}/assets/logo.ico** 추가하기

### 9. 오류 해결

- CSS깨짐 오류 발생
  - **_config.yml**에서 **baseurl**수정
  ```
  baseurl: ""
  ```
  - `git add . 그리고 git commit -m "아무말"`한 후 현재 디렉토리에서 아래 코드 실행
  ```
  gem cleanup
  bundle update
  bundle install
  jekyll serve
  ```

- 404Error 오류 발생
  - **_config.yml**에서 아래 코드를 수정
  ```
  # theme: jekyll-theme-so-simple
  remote_theme: mmistakes/so-simple-theme
  ```
