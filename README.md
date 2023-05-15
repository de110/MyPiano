# MyPiano
2020-2 LabView 프로젝트

LabView 와 My Rio를 이용한 피아노 제작 팀 프로젝트

개발 인원 - 3명

1. 하드웨어 구성
    
    1.1 전체 피아노 구성
    
    <img width="550" alt="피아노_구성" src="https://user-images.githubusercontent.com/67581448/233924362-d824ec7e-8db9-4420-a51b-0a6ff3a763ba.png">
    

    1.2 사용한 모듈

      1) 회전 스위치 ( Rotary Switch )
      
      <img width="150" alt="로터리_스위치" src="https://user-images.githubusercontent.com/67581448/233924481-4ada5e99-64b8-4d50-a7f9-20b9d1827681.png">

        - 볼륨(10~100), 옥타브(총 8옥타브) 조절에 사용
        - 회전을 통해 값을 조절한다.

      2)  택트 스위치 ( Tact Switch )

        - 건반과 재생 버튼에 연결되어, 연주와 녹음된 곡 재생에 사용

      3)  슬라이드 스위치( Slide Switch )

        - 녹음 버튼에 연결된다.
        - 스위치가 오른쪽에 위치해 있을 때 On, 왼쪽에 위치할 경우 Off
        - 스위치 상태를 On으로 유지시키는 동안 연주곡이 녹음된다.
        - 연주를 끝내고 스위치 상태를 Off로 설정하면 녹음이 종료된다.

      4)  댐퍼 페달 ( Damper Pedal )

        - 피아노에서, 건반에서 손을 뗀 후에도 연주한 음을 지속시켜 주는 서스테인(Sustain) 페달의 기능을 구현
        - 페달이 눌린 순간을 기준으로, 연주된 음을 유지시켜서 음과 음 사이를 자연스럽게 연결해준다.

    1.3 피아노 건반

      백건 21개, 흑건 15개로 총 36개의 건반으로 피아노를 구성했다.
      
      MyRio의 DIO 개수 제한으로 인해, 36개의 택트 스위치를 각각 연결할 수 없기 때문에 입력 회로를 6x6 Array로 구성하여 포트 개수의 한계를 해결하였다.

      1) 실제 회로 구성 모습
      
      <img width="550" alt="회로_하드웨어" src="https://user-images.githubusercontent.com/67581448/233924634-ac1b08ae-9e0d-47b9-a2c3-e7fd757bf387.png">    
        
        앞에서부터 6개의 스위치 순서대로 회로를 이어서 출력을 구성하고, 각 묶음의 같은 번째 자리의 스위치끼리 회로를 이어서 입력을 구성했다.

      2) 회로도
      
      <img width="550" alt="회로도_1" src="https://user-images.githubusercontent.com/67581448/233924586-970218b2-de3f-4d29-97c4-7df249115e6f.png">
        
        - DIO0~5: 입력, DIO6~11: 출력

      3) 건반 입력 인식 방법
      
      <img width="550" alt="회로도_2" src="https://user-images.githubusercontent.com/67581448/233924599-8fe4b83d-f5cf-435d-bf56-2500cbfd754b.png">

        e.g. S5의 값을 찾는 경우
        
        S5번째 버튼을 누르면 DIO7 이 High로 활성화된다.
        DIO0~DIO5까지의 값을 순차적으로 High로 올려 검사하여, 각 행/열이 모두 High인 값을 검색한다.


2. 소프트웨어 구성

    2.1 프론트 패널(Front Panel) 전체 구성
  
    <img width="550" alt="Front_Panel" src="https://user-images.githubusercontent.com/67581448/233928412-73fcd069-c426-4a25-abef-652808cf0cef.png">
    
        1) 녹음 저장 및 불러오기 파일 위치 지정
    
    <img width="550" alt="Record_Save" src="https://user-images.githubusercontent.com/67581448/233928448-553dafd8-b5b6-4b73-a3b5-2d4236fd4170.png">
        
            - 녹음한 연주곡을 저장하고, 재생하기 위한 파일의 위치를 지정해준다.
        
        2) 녹음 시작 / 종료, 재생 버튼
        
            - 하드웨어의 슬라이드 스위치(1.2-3)와 택트 스위치(1.2-2)의 재생 버튼과 연결
            - Record : 하드웨어와 동일하게 스위치가 오른쪽에 위치할 경우 On, 왼쪽에 위치할 경우 Off
            - Play : 재생을 위한 택트 스위치를 누를 경우 Play 버튼이 활성화, 녹음된 곡이 재생된다.

        3) 볼륨(Volume), 옥타브(Octave) 조절 

            - 하드웨어의 회전 스위치(1.2-1)와 연결

        4) 건반

            - 건반 역할을 하는 택트 스위치(1.2-2) 와 연결
            - 하드웨어에서 눌린 건반의 위치를 동시에 화면에서도 표시한다.


    2.2 Block Diagram
        
        1) 건반 입력
     
     <img width="550" alt="눌린_피아노_버튼" src="https://github.com/de110/MyPiano/assets/67581448/3383c351-8df7-4a8d-9a9f-b19cc1be449a">
        
        - 6by6 배열로 연결된 풀업 스위치는 Flat sequence를 사용하여 빠르게 전체 값를 읽어준다.
        - 풀업 스위치의 기본 값은 True여서 입력 값이 False일 때 출력 값을 확인한다.

        2) 옥타브
        
     <img width="550" alt="3 옥타브_0" src="https://github.com/de110/MyPiano/assets/67581448/93b040c1-f76a-4af9-bba8-05b670c5a1bd">
         
         - 스위치를 바꿀 때마다 바뀌는 값에 맞춰서 Default, 2100, 2400, 2700, 3000, 3200, 3300 까지 총 7개의 케이스를 둔다.
         - 2의 거듭제곱 연산을 통해서 옥타브를 조절한다. 
         - 각 배열의 첫번째 값은 다중 입력할 때 아무것도 눌리지 않았을 때의 주파수 값 0으로 지정한다.

        3) 건반 다중 입력
        
    <img width="224" alt="4 피아노_다중_입력" src="https://github.com/de110/MyPiano/assets/67581448/a2bd2cba-44a0-41b7-a7d1-3bd9f699af3e">
    <img width="160" alt="4 피아노_다중_입력2" src="https://github.com/de110/MyPiano/assets/67581448/c1d8c7e0-f485-4e29-a177-5605eb8999a2">
        
         - Local variable로 피아노 키보드의 bool값을 받아오고 하나의 배열로 만든다.
         - bool 배열은 풀업(PULL-UP)형태로 되어 있어서 not을 통해 건반을 누른 부분을 True로 만든다.
         - 만약 True가 있다면, 새로운 배열에 해당하는 인덱스를 추가하고, False면 아무것도 하지 않는다.
         - 0번째 위치에 해당하는 해당하는 인덱스는 주파수1의 인덱스가 되고, 1번째 위치에 해당하는 인덱스는 주파수2의 인덱스가 된다. 
         - 주파수 배열에서 해당하는 인덱스 값을 찾아서 주파수1과 주파수 2에 각각 넣어준다.

        4) 볼륨

    <img width="119" alt="5 로터리_스위치1" src="https://github.com/de110/MyPiano/assets/67581448/74359366-3334-40fa-8293-5e4a637d7415">
    <img width="160" alt="5 로터리_스위치2" src="https://github.com/de110/MyPiano/assets/67581448/615f4ece-3bee-4947-a464-6f625105fe7a">
        
         - 로터리 음량 스위치로 받아온 값을 volume에 넣어준다. 값은 다음과 같다.
    
    <img width="136" alt="image" src="https://github.com/de110/MyPiano/assets/67581448/c7e07186-21ab-4832-b8fb-cc7f11c8149f">


    2.3 FPGA
        
