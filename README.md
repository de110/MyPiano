# MyPiano
2020-2 LabView 프로젝트

LabView 와 My Rio를 이용한 피아노 제작 팀 프로젝트

개발 인원 - 3명

1. 하드웨어 구성
    
    1.1 전체 피아노 구성
    
    <img width="548" alt="피아노_구성" src="https://user-images.githubusercontent.com/67581448/233924362-d824ec7e-8db9-4420-a51b-0a6ff3a763ba.png">
    

    1.2 사용한 모듈

      a. 회전 스위치 ( Rotary Switch )
      
      
      <img width="199" alt="로터리_스위치" src="https://user-images.githubusercontent.com/67581448/233924481-4ada5e99-64b8-4d50-a7f9-20b9d1827681.png">


        - 볼륨(10~100), 옥타브(총 8옥타브) 조절에 사용
        - 회전을 통해 값을 조절한다.

      b.  택트 스위치 ( Tact Switch )

        - 건반과 재생 버튼에 연결되어, 연주와 녹음된 곡 재생에 사용

      c.  슬라이드 스위치( Slide Switch )

        - 녹음 버튼에 연결된다.
        - 스위치가 오른쪽에 위치해 있을 때 On, 왼쪽에 위치할 경우 Off
        - 스위치 상태를 On으로 유지시키는 동안 연주곡이 녹음된다.
        - 연주를 끝내고 스위치 상태를 Off로 설정하면 녹음이 종료된다.

      d.  댐퍼 페달 ( Damper Pedal )

        - 피아노에서, 건반에서 손을 뗀 후에도 연주한 음을 지속시켜 주는 서스테인(Sustain) 페달의 기능을 구현
        - 페달이 눌린 순간을 기준으로, 연주된 음을 유지시켜서 음과 음 사이를 자연스럽게 연결해준다.

    1.3 피아노 건반

      백건 21개, 흑건 15개로 총 36개의 건반으로 피아노를 구성했다.
      
      MyRio의 DIO 개수 제한으로 인해, 36개의 택트 스위치를 각각 연결할 수 없기 때문에 입력 회로를 6x6 Array로 구성하여 포트 개수의 한계를 해결하였다.

      a. 실제 회로 구성 모습
      
      <img width="641" alt="회로_하드웨어" src="https://user-images.githubusercontent.com/67581448/233924634-ac1b08ae-9e0d-47b9-a2c3-e7fd757bf387.png">    
        
        앞에서부터 6개의 스위치 순서대로 회로를 이어서 출력을 구성하고, 각 묶음의 같은 번째 자리의 스위치끼리 회로를 이어서 입력을 구성했다.

      b. 회로도
      
      <img width="614" alt="회로도_1" src="https://user-images.githubusercontent.com/67581448/233924586-970218b2-de3f-4d29-97c4-7df249115e6f.png">
        
        - DIO0~5: 입력, DIO6~11: 출력

      c. 건반 입력 인식 방법
      
      <img width="433" alt="회로도_2" src="https://user-images.githubusercontent.com/67581448/233924599-8fe4b83d-f5cf-435d-bf56-2500cbfd754b.png">

        e.g. S5의 값을 찾는 경우
        
        S5번째 버튼을 누르면 DIO7 이 High로 활성화된다.
        DIO0~DIO5까지의 값을 순차적으로 High로 올려 검사하여, 각 행/열이 모두 High인 값을 검색한다.


2. 소프트웨어 구성

    2.1 프론트 패널(Front Panel) 전체 구성
  
    <img width="635" alt="Front_Panel" src="https://user-images.githubusercontent.com/67581448/233928412-73fcd069-c426-4a25-abef-652808cf0cef.png">
    
    a. 녹음 저장 및 불러오기 파일 위치 지정
    
    <img width="160" alt="Record_Save" src="https://user-images.githubusercontent.com/67581448/233928448-553dafd8-b5b6-4b73-a3b5-2d4236fd4170.png">
        
        - 녹음한 연주곡을 저장하고, 재생하기 위한 파일의 위치를 지정해준다.
        
    b. 녹음 시작 / 종료, 재생 버튼
        
        - 하드웨어의 슬라이드 스위치(1.2-c)와 택트 스위치(1.2-b)의 재생 버튼과 연결
        - Record : 하드웨어와 동일하게 스위치가 오른쪽에 위치할 경우 On, 왼쪽에 위치할 경우 Off
        - Play : 재생을 위한 택트 스위치를 누를 경우 Play 버튼이 활성화, 녹음된 곡이 재생된다.
    
    c. 볼륨, 옥타브 조절 
        
        - 하드웨어의 회전 스위치(1.2-a)와 연결
    
    d. 건반
        
        - 건반 역할을 하는 택트 스위치(1.2-b) 와 연결
        - 하드웨어에서 눌린 건반의 위치를 동시에 화면에서도 표시한다.
