# 00. matplotlib 한글처리

### 기본설정
- matplotlib을 사용할 때 기본적으로 한글이 깨져서 나온다.
- 해결방법
  1. 설정파일 변경
  ```python
  # 폰트 cache파일 삭제
  import matplotlib as mpl
  import matplotlib.font_manager as fm
  # cache 파일 조회
  # 다음 실행 결과로 나온 경로의 파일을 삭제한다. 
  mpl.get_cachedir()

  # 전체 폰트 조회
  for f in fm.fontManager.ttflist:
  print(f.name, f.fname, sep='::::')  # 폰트이름, 폰트파일경로
  # 원하는 폰트명 조회
  [(f.name,f.fname) for f in fm.fontManager.ttflist if 'malg' in f.name.lower()]  
  #mac : AppleGothic
  # 설정시 폰트 이름을 사용
  설정파일 경로찾기: `matplotlib.matplotlib_fname()`
  matplotlib 관련 전역 설정들을 찾아 바꿔준다.
  - 폰트 관련 설정
  # font.family:Malgun Gothic
  # font.size:12
  # xtick.labelsize:12
  # ytick.labelsize:12 
  # axes.labelsize:12  
  # axes.titlesize:20
  # axes.unicode_minus:False

  # 설정 값 경로
  import matplotlib as mpl
  mpl.matplotlib_fname() # 로 찾아 조회 후 해당 파일내에서 주석을 풀고 위의 설정으로 각각 찾아 변경
  ```
  2. 설정 변경하는 코드 작성
  ```python
  import matplotlib as mpl
  from matplotlib import font_manager as fm

  # 한글 폰트 설정 - 맑은 고딕체 찾아 변수에 담기
  font_name = fm.FontProperties(fname="c:/Windows/Fonts/malgun.ttf").get_name()
  # 'Malgun Gothic'

  COLOR = 'white'
  mpl.rcParams['text.color'] = COLOR
  mpl.rcParams['axes.labelcolor'] = COLOR
  mpl.rcParams['xtick.color'] = COLOR
  mpl.rcParams['ytick.color'] = COLOR

  mpl.rcParams["font.family"] = font_name
  mpl.rcParams["font.size"] = 15
  mpl.rcParams['xtick.labelsize'] = 12
  mpl.rcParams['ytick.labelsize'] = 12
  mpl.rcParams['axes.labelsize'] = 15
  # tick의 음수기호 '-' 가 깨지는 것 처리
  mpl.rcParams['axes.unicode_minus'] = False
  ```
- - -

# 01. Matplotlib 개요

  ### 1. Matplotlib
  - 데이터의 시각화를 위한 `파이썬` 패키지
  - MATLAB과 유사한 사용자 인터페이스
  - 구성요소
    - figure : 전체 그래프가 위치할 기본 틀
    - axes(subplot) : 하나의 그래프를 그리기 위한 공간
      - figure에 한개 이상의 axes(subplot)로 구성해서 각 axes에 그래프를 그린다.
    - axis : 축 / axis label (x, y) : 축의 레이블(설명)
    - ticks : 축선의 표시(Major, Minor)
    - title : 플롯 제목   
    - legend (범례) : 하나의 axes내에 여러 그래프를 그린 경우 그것에 대한 설명
    ```python
    # %matplotlib inline
    # 위는 '%'로 시작하는 구문은 주피터노트북 명령어/안해도 상관없음
    # %matplotlib qt

    # 어두운 테마에서 pyplot 사용할 때 색상 조정
    plt.style.use(['dark_background'])

    import matplotlib.pyplot as plt
    import numpy as np

    plt.style.use(['dark_background'])
    fig = plt.figure(figsize=(15,7), facecolor='gray') #facecolor: figure의 배경색
    axes1 = fig.add_subplot(1,2,1)
    axes2 = fig.add_subplot(1,2,2)

    axes1.plot([1,2,3,4,5], [10,20,30,40,50], label='line1')
    axes1.plot([1,2,3,4,5], [50,40,30,20,10], label='line2')
    axes2.scatter(np.random.randint(100, size=50), np.random.randint(100, 200, size=50), color='r')

    fig.suptitle('Example of Plot', size=25, color='blue') #size: 폰트크기, color: 글자색
    axes1.set_title("PLOT 1", size=20)
    axes2.set_title("Plot 2", size=20)

    axes1.set_xlabel("X축", size=15)
    axes1.set_ylabel("Y축", size=15)
    axes2.set_xlabel("가격1", size=15)
    axes2.set_ylabel('가격2', size=15)

    axes1.legend()
    axes1.grid(True)

    plt.show()
    ```
    
  ### 2. 그래프 그리기
  1. matplotlib.pyplot 모듈 import
    - 2차원 그래프(axis가 두개인 그래프)를 그리기 위한 함수를 제공하는 모듈
    - 관례적으로 plt를 별칭(alias)으로 사용
    - `import matplotlib.pyplot as plt`
  2. 그래프를 그린다.
      - 2가지 방식
          - pyplot 모듈 이용
          - Figure와 Axes 객체 이용
  3. 그래프에 필요한 설정을 한다.
  4. 화면에 그린다.
      - 지연 랜더링(Deferred rendering) 메커니즘
      - 마지막에 `pyplot.show()` 호출 시 그래프를 그린다.
          - 주피터 노트북 맨 마지막 코드에 `;`를 붙이는 것으로 대체 가능
  
  
  ### 3. 2가지 그래프 작성방식
  1) pyplot 모듈 이용
  ```python
  import matplotlib.pyplot as plt

  # figure의 크기
  plt.figure(figsize=(5,10)) # 가로, 세로 크기 - inch 단위
  # subplot 지정
  plt.subplot(2,1,1) # 그래프를 그릴 axes(subplot)을 지정
  # 그래프 그리기
  plt.plot([1,2,3],[10,20,30])
  # 추가 설정
  plt.title('첫번째')

  # subplot지정
  plt.subplot(2,1,2)
  # 그래프 그리기
  plt.scatter([1,2,3],[10,20,30])
  # 추가 설정
  plt.title('두번째')

  plt.show()
  ```
  2) Figure와 Axes객체를 이용
  - figure객체에 axes를 추가하는 형태
  - `figure.add_subplot()`메소드와 `pyplot.subplots()`함수를 이용하는 두 방법.
    - (1) `figure.add_subplot()`메소드
    ```python
    fig2 = plt.figure(figsize=(10,7))
    ax1 = fig2.add_subplot(1,3,1)
    ax2 = fig2.add_subplot(1,3,2)
    ax3 = fig2.add_subplot(3,3,3)
    ax4 = fig2.add_subplot(3,3,6)
    ax5 = fig2.add_subplot(3,3,9)

    for i, ax in enumerate([ax1, ax2, ax3, ax4, ax5], start=1):
        ax.text(.5,.5, f'ax {i}', fontsize=30)
        # subplot의 title을 지정
        ax.set_title(f'ax {i}') # pyplot모듈과는 메소드이름이 다르다
        # x, y축 label
        ax.set_xlabel('X')
        ax.set_ylabel('Y')

    plt.tight_layout() # subplot 배치를 자동으로 처리
    plt.show()
    ```
    - (2) `pyplot.subplots()`함수
    ```python
    import matplotlib.pyplot as plt
    fig, axes = plt.subplots(2,2, figsize=(10,10)) # 행수, 열수
    fig.suptitle('Figure 제목', size=40)
    print(type(axes), axes.shape, type(axes[0,0]))

    ax1, ax2, ax3, ax4 = axes.flatten()
    # -> 외부 모듈에서 넘파이 객체를 받아올때도 import numpy를 할 필요없음
    ax1.scatter([1,2,1,2,6],[7,3,4,2,1])
    ax1.set_title('[0,0]')
    # axes[0,0].scatter([1,2,1,2,6],[7,3,4,2,1])
    # axes[0,0].set_title('[0,0]')
    ax2.plot([1,2,3],[1,2,3], label='line1')
    ax2.plot([1,2,3],[3,2,1], label='line2')
    ax2.grid(True)
    ax2.legend()

    for i, ax in enumerate([ax1, ax2, ax3, ax4], start=1):
        x, y = .5, .5
        if ax == ax1:
            x, y = 5, 5
        ax.text(x, y, f'ax{i}', fontsize=20)

    plt.show()
    ```


  ### 4. 그래프 색상, 스타일 설정
  1) 색 지정
  - HTML 컬러문자열
    - #으로 시작하며 RGB의 성분을 16진수로 표현
    - #RRGGBB 또는 #RRGGBBAA
    - #FF0000, #00FF00FA
  - 0 ~ 1 사이 실수로 흰식과 검정색 사이의 회색조를 표시
      - 0: 검정, 1: 흰색
  ```python
  plt.figure(figsize=(7,7))#, facecolor='m')
  # plt.plot([1,2,3], [10,20,30], color='#C878F4')
  # plt.plot([1,2,3], [10,20,30], color='##FF000A0')
  plt.plot([1,2,3], [10,20,30], color='.9') # 0(검정) ~ 1(흰색) 실수 - 회색조
  plt.show()
  ```
  2) Style
  - Style: 그래프의 여러 시각효과들을 미리 설정해 놓은 것
  - matplotlib는 다양한 스타일들을 미리 정의해 놓고 있다.
      - `plt.style.use()` 함수 이용해 지정
      - 스타일 초기화
      ```python
      import matplotlib as mpl
      mpl.rcParams.update(mpl.rcParamsDefault)
      ```
  - 구문
  ```python
  # plt.style.use(['dark_background'])
  import numpy as np
  x = np.linspace(1,10, num=100)

  # plt.style.use('ggplot')
  plt.style.use('seaborn')

  plt.figure(figsize=(7,7))
  plt.plot(x, x+3)
  plt.plot(x, x+2)
  plt.plot(x, x+1)
  plt.plot(x, x)

  plt.show()
  ```
