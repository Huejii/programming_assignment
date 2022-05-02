# Programming_1
## < 학급 성적 계산 프로그램 >
###### - 경제학과 201821860 채희주
---

알고리즘 정의
---

<img src="https://user-images.githubusercontent.com/94972402/166223016-ef15e372-4b97-4ad0-8d37-1856534e3be2.png" width="600">

코드
---

```c
#include <stdio.h>

/******************* 기호상수 정의 *******************/
#define STUDENTS 3   // 학급별 학생 수
#define SUBJECTS 4   // 과목 수

/******************* 함수 원형 *******************/
void score_input(double a[STUDENTS][SUBJECTS]);        // 정의: 7. 성적 입력 함수
double* print_sum_avg(double a[STUDENTS][SUBJECTS]);   // 정의: 8. 성적 산출 및 출력 함수
void print_total_avg(double* b, int a);                // 정의: 9. 전체 학급 과목별 평균 산출 및 출력 함수
```

기호상수와 함수는 위와 같이 선언하였다. 기호상수는 `SUTDENTS`는 학급별로 학생 수가 3, `SUBJECTS`는 과목 수가 4인 것을 의미한다. 

함수는 정의부에서 코드와 함께 자세하게 설명할 예정으로, 간단히 설명하면

7. `score_input` : 학급별로 성적을 입력한다.

8. `print_sum_avg` : 7번 함수에서 입력받은 성적 배열을 토대로 학급의 총점과 평균을 계산하여 출력한다. 함수 내 전체 학급의 과목별 성적을 합산하는 **static** 유형의 1차원 배열인 `cumSum`을 선언하여 누적하고, 이 배열의 주소를 반환값으로 한다. 

9. `print_total_avg` : 8번함수에서 반환된 배열 주소값을 토대로 전체 학급의 과목별 평균을 산출하고 출력한다.


```c
/* main함수 시작 */
int main(void)
{
/******************* 학급 SETUP *******************/
	int classes;                                                 // 1. 학급 수 입력받을 변수 선언

	class_input: printf("1. 학급의 개수를 입력해주세요(1~3): ");  // 2. 학급 수 키보드 입력
	scanf_s("%d", &classes);

	if (classes < 1 || classes > 3)                              // 3. 학급 수가 1 ~ 3개 아니면 오류
	{
		printf("\n※ 학급은 1~3개까지 입력 가능합니다. 다시 입력해 주세요.\n\n");  // 오류 메세지
		goto class_input;                                    // < 2. 학급 수 키보드 입력 >으로 다시 이동
	}
```
학급 수를 사용자에게 입력받는다. 입력받은 int형 변수 `classes`가 1~3가 아닐 시, 오류 메세지를 출력하고 `goto`함수를 통해 다시 학급 수를 입력하는 함수로 되돌아간다.

```c
	double class0[STUDENTS][SUBJECTS] = {0.0};;                // 4. 학급별 2차원 배열(학생수X과목수) 생성
	double class1[STUDENTS][SUBJECTS] = {0.0};;
	double class2[STUDENTS][SUBJECTS] = {0.0};;
```
학급이 최대 3개까지 있을 수 있으므로, 3개의 2차원 배열 변수를 선언하고 초기화하였다.
```c
	// 5. <8. 성적 산출 및 출력 함수>에서 선언한 < 전체학급의 과목별 총점 배열 > 반환값을 대입할 포인터 변수
	double* totalScore = 0;
```
성적 산출 및 출력함수에서 선언한 static한 1차원 배열값을 반환받을 포인터변수 `totalScore`를 선언하였다. 이는 <9. 전체 학급 과목별 평균 산출 및 출력 함수)의 parameter가 될것이다.


```c
/******************* 성적 산출 *******************/

	// 6. 입력받은 학급수에 따른 switch문: 성적입력, 총점&평균 출력
	switch (classes)
	{
	case 3: //2반
		printf("\n\n----< 2반 성적 입력 > ----\n\n");
		score_input(class2);      // < 7. 성적 입력(키보드) 함수 > 호출 : 학급별 성적 입력
		print_sum_avg(class2);    // < 8. 학급별 총점&평균 성적 출력 함수 > 호출 / 리턴값: 과목별 총점 누적 배열(주소)

	case 2: //1반
		printf("\n\n----< 1반 성적 입력 >---- \n\n");
		score_input(class1);
		print_sum_avg(class1);

	case 1: //0반
		printf("\n\n----< 0반 성적 입력 >---- \n\n");
		score_input(class0);
		totalScore = print_sum_avg(class0);  // 누적된 전체학급의 과목별 총점 배열을 리턴하여 포인터변수에 대입
	}
```
입력받은 학급 수인 `classes`에 따라 `switch`문의 case를 나누었다. `case n`은 'class(n-1)'의 성적 입력과 총점&평균 성적을 출력할 함수를 호출한다.

`break`를 걸지 않음에 따라, `case 3`의 경우를 예로 들 때 `case 2` 실행된 이후 차례대로 `case 2`, `case 1`이 실행되는 식으로 진행된다. 이에 따라 뒷반의 case부터 실행된다.

`case 1`에서는 최종적으로 누적된 <8. 학급별 총점&평균 성적 출력 함수>에서 선언한 과목별 총점 배열을 포인터변수 `totalScore`에 대입한다. 

```c
	// 전체 학급의 과목별 평균 출력
	print_total_avg(totalScore, classes); // < 9. 전체 학급 과목별 평균 출력 함수 > 호출, input: 과목별 총점 배열 포인터, 학급 수

	return 0;
}
/* main 함수 끝 */
```

switch문이 끝나고, <9. 전체학급의 과목별 평균 출력 함수>를 호출하여 앞서 반환받은 `totalScore`와 학급수 `classes`를 parameter로 넣어 과목별 총평균을 출력한다. 

코드 中 함수정의
---

```c
/******************* 함수 정의 *******************/
// 7. 성적 입력 함수 정의 : 학생X과목별 키보드 입력 
void score_input(double a[STUDENTS][SUBJECTS])  // 학급별 성적 배열
{
	;
	for (int i = 0; i < STUDENTS; i++)      // for문을 이용하여 키보드로 배열값 입력
	{
		printf("\n- 학생%d의 성적 입력\n\n", i);
		for (int j = 0; j < SUBJECTS; j++)
		{
			printf("과목 %d의 성적: ", j);
			scanf_s("%lf", &a[i][j]);
		}
	}
}
```
7. score_input: 학급별로 성적을 입력한다. 입력은 사용자의 키보드로 입력받는다. 이 때, parameter값은 학생과 과목간의 이차원 배열이다. 배열값을 입력하기 위한 중접for문으로, 외부 For는 과목 수 `SUBJECTS`만큼, 내부For문은 학생 수 `STUDENTS`만큼 돈다. 따라서 다음과 같은 순서대로 입력된다.

내부For \ 외부For|과목0|과목1|과목2|과목3
--|--|--|--|--
학생0|①|④|⑦|⑩
학생1|②|⑤|⑧|⑪
학생2|③|⑥|⑨|⑫

```c
// 8. 성적 산출 함수 정의: 학급별 총점 & 평균 출력
double *print_sum_avg(double a[STUDENTS][SUBJECTS]) // 학급별 성적 배열, 과목별 전체학급 성적 배열
{
	static double cumSum[SUBJECTS] = { 0.0 }; // 저장 유형 지정자 static > 전체 학급의 과목별 성적 누적
	double total = 0;                    // 학급별 총점 변수
	for (int j = 0; j < SUBJECTS; j++)
	{
		double sum = 0;              // 과목별로 학생성적을 누적하기 위해 큰 for문 돌 때마다 초기화
		for (int i = 0; i < STUDENTS; i++)
		{
			sum += a[i][j];      // 과목별 학급 내 성적 누적
		}
		cumSum[j] += sum;                 // 과목별 전체학급의 성적 누적
		total += sum;                //  학급 내 성적 누적
	}
	printf("\n▶ 총점은 %f, 평균은 %.2f입니다.\n\n", total, total / (STUDENTS * SUBJECTS));
	return cumSum;
}
```


```c
// 9. 전체 학급의 과목별 평균 출력 함수 정의
void print_total_avg(double* b, int a)               // 과목별 전체학급 성적 배열, 학급수
{
	double avg;                                  // 과목별 총점으로 평균을 계산하여 넣을 <과목별 전체 평균 변수> 선언
	printf("\n\n===== << 전체 학급의 과목별 평균 >> =====\n\n");
	for (int j = 0; j < SUBJECTS; j++)
	{
		avg = *(b + j/8) / (a * STUDENTS);   // 과목별 전체학생의 평균 점수 = (전체학급의 과목별 점수)/(학급수X학생수)
		printf("       과목 % d의 총 평균 : % .2f\n", j, avg);
	}
}
```
