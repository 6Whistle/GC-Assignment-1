GPU 컴퓨팅 Assignment 1

이름 : 이준휘

학번 : 2018202046

교수 : 공영호 교수님

강의 시간 : 월 수

1.  Introduction

해당 과제는 주어진 CUDA memecpy 코드(Assignment1cuda.cu)에서 필요한
부분을 작성하여 코드를 작성한다. 완성된 코드와 GPU를 사용하지 않은 기본
코드(Assignment1.cpp)와 비교하여 동일한 출력이 나오는지 확인한다. 이를
토대로 현재 GPU 컴퓨팅을 위한 환경이 제대로 설정되었는지 알 수 있다.

2.  Approach

![](media/image1.png){width="6.268055555555556in"
height="3.9194444444444443in"}

해당 코드는 Assignment1cuda.cu다. 코드에서 cudaMalloc() 함수 2개와
cudaMemcpy() 함수가 3개의 인자 부분이 비어있으며, 해당 부분을 채워서
코드를 완성해야 한다.

![](media/image2.png){width="6.268055555555556in"
height="3.986111111111111in"}

우선 cudaMalloc() 함수를 먼저 설명한다. cudaMalloc()은 기존 C언어의
malloc() 함수와 같이 동적으로 메모리를 할당하는 것이다. 해당 함수는
malloc()과 쓰는 방법이 약간 다르다. malloc()에서는 인자로 단순히
크기만을 할당하고 반환되는 주소를 포인터에 대입하는 방식이다. 하지만
cudaMalloc()은 인자에 포인터의 주소를 추가하여 이를 참조한다. 따라서
처음 cudaMalloc()은 d_a에 sizeof(int) \* SIZE 크기의 메모리를 동적
할당하고 싶으므로 cudaMalloc(&d_a, SIZE\*sizeof(int));를 수행하며 d_b에
관한 메모리 할당도 마찬가지로 cudaMalloc(&d_b, SIZE\*sizeof(int));를
수행한다.

다음으로 cudaMemcpy() 함수를 설명한다. 해당 함수는 서로 다른 메모리 간의
데이터 이동을 원활하게 수행하도록 도와준다. 해당 함수의 parameter로는
옮길 위치, 원본 위치, 이동시킬 데이터 크기, 이동 옵션 순으로 있다. 이 때
이동 옵션은 enum 형태로 정의되었으며 만약 RAM -\> VRAM(Host -\>
Device)으로 이동시킬 경우에는 enum cudaMemcpyHostToDevice를 사용하며,
반대의 경우 cudaMemcpyDeviceToHost를 사용한다. 27\~28줄의 cudaMemcpy()는
a와 b의 데이터를 device의 d_a와 d_b로 옮겨야 한다. 때문에 해당 함수는
각각 cudaMemcpy(d_a, a, SIZE\*sizeof(int), cudaMemcpyHostToDevice);
cudaMemcpy(d_b, b, SIZE\*sizeof(int), cudaMemcpyHostToDevice);를
수행한다. 30번째 줄에서는 d_b의 데이터를 b로 옮기는 작업을 수행한다. 이
때에는 cudaMemcpy(b, d_b, SIZE\*sizeof(int), cudaMemcpyDeviceToHost);를
수행하여 device에서 host로 옮긴다.

3.  Result

![](media/image3.png){width="6.268055555555556in"
height="3.917361111111111in"}

![](media/image4.png){width="6.268055555555556in"
height="3.917361111111111in"}

첫 번째 화면은 기존 cpp 코드를 g++환경에서 수행한 모습이다. 해당 결과를
살펴보면 1\~10까지 저장된 b\[10\] 배열을 출력하는 코드다. 다음 화면은
colab을 SSH를 통해 vscode로 원격 연결하여 작업을 수행한 모습이다.
터미널에서 \$nvcc -o Assignment1cuda Assignment1cuda.cpp를 통해 코드를
컴파일하고 실행하였을 때 cpp 코드와 동일한 결과를 출력하는 모습을
보인다. 이를 통해 해당 코드가 정상적으로 작성되었음을 확인하였다.

4.  Consideration

해당 과제를 통해 cuda 작업 환경이 어떻게 이루어져 있는지 확인할 수
있었다. nvcc 명령어를 통해 .cu 파일을 컴파일 하는 방법을 익힐 수 있었다.
또한 .cu 파일의 코드가 기존 c++와 매우 유사한 모습을 보인다는 것 또한 알
수 있었다. 마지막으로 cudaMalloc()와 cudaMemcpy() 함수가 어떠한
parameter를 가지고 있는지 확인할 수 있었고, 특히 cudMemcpy() 함수에서
Host와 Deivce간 데이터를 복사하는 방법을 익힐 수 있었다.

5.  Reference

-   강의자료만을 참고
