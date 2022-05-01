# programming_assignment

#include <stdio.h>

```c
/******************* 기호상수 정의 *******************/
#define STUDENTS 3  // 학급별 학생 수
#define SUBJECTS 4  // 과목 수
```
```c
/******************* 함수 원형 *******************/
void score_input(double a[STUDENTS][SUBJECTS]);       // 정의: 7. 성적 입력 함수
double* score_sum_avg(double a[STUDENTS][SUBJECTS]);      //정의: 8. 성적 산출 및 출력 함수
void total_subject_avg(double* b, int a);      // 정의: 9. 전체 학급 과목별 평균 산출 및 출력 함수
```

```c
/* main함수 시작 */
int main(void)
{
	/******************* 학급 SETUP *******************/

	// 1. 학급 수 입력받을 변수 선언
	int classes;

	// 2. 학급 수 키보드 입력
	class_input: printf("1. 학급의 개수를 입력해주세요(1~3): ");
	scanf_s("%d", &classes);

	// 3. 학급 수가 1 ~ 3개 아니면 오류
	if (classes < 1 || classes > 3)
	{
		printf("\n※ 학급은 1~3개까지 입력 가능합니다. 다시 입력해 주세요.\n\n"); // 오류 메세지
		goto class_input;   // < 2. 학급 수 키보드 입력 >으로 이동
	}
```
```c
	// 4. 학급별 배열 생성(학급수에 따라 안쓰는 배열이 생기므로 초기화X)
	double class0[STUDENTS][SUBJECTS];  // 2차원 배열
	double class1[STUDENTS][SUBJECTS];
	double class2[STUDENTS][SUBJECTS];
```
```c
	// 5. 성적 입력 함수에서 선언한 < 전체학급의 과목별 총점 배열 > 반환값을 대입할 포인터 변수
	double* p = 0;
```

```c
	/******************* 성적 산출 *******************/


	// 6. 입력받은 학급수에 따른 switch문: 성적입력, 총점&평균 출력
	switch (classes)
	{
	case 3: //2반
		printf("\n\n----< 2반 성적 입력 > ----\n\n");
		score_input(class2);      // < 7. 성적 입력(키보드) 함수 > 호출 : 학급별 성적 입력
		score_sum_avg(class2);      // < 8. 학급별 총점&평균 성적 출력 함수 > 호출 / 리턴값: 과목별 총점 누적 배열(주소)

	case 2: //1반
		printf("\n\n----< 1반 성적 입력 >---- \n\n");
		score_input(class1);
		score_sum_avg(class1);

	case 1: //0반
		printf("\n\n----< 0반 성적 입력 >---- \n\n");
		score_input(class0);
		p = score_sum_avg(class0); // 누적된 전체학급의 과목별 총점 배열을 리턴하여 포인터변수에 대입
	}
```
```c
	// 전체 학급의 과목별 평균 출력
	total_subject_avg(p, classes);      // < 9. 전체 학급 과목별 평균 출력 함수 > 호출, input: 과목별 총점 배열 포인터, 학급 수

	return 0;
}
/* main 함수 끝 */
```




```c
/*************************************************/
/******************* 함수 정의 *******************/

// 7. 성적 입력 함수 정의 : 학생X과목별 키보드 입력 
void score_input(double a[STUDENTS][SUBJECTS]) // 학급별 성적 배열
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


```c
// 8. 성적 산출 함수 정의: 학급별 총점 & 평균 출력
double *score_sum_avg(double a[STUDENTS][SUBJECTS]) // 학급별 성적 배열, 과목별 전체학급 성적 배열
{
	static double b[SUBJECTS] = { 0.0 }; // 저장 유형 지정자 static > 전체 학급의 과목별 성적 누적
	double total = 0;           // 학급별 총점 변수
	for (int j = 0; j < SUBJECTS; j++)
	{
		double sum = 0;         // 과목별로 학생성적을 누적하기 위해 큰 for문 돌 때마다 초기화
		for (int i = 0; i < STUDENTS; i++)
		{
			sum += a[i][j];     // 과목별 학급 내 성적 누적
		}
		b[j] += sum;            // 과목별 전체학급의 성적 누적
		total += sum;           //  학급 내 성적 누적
	}
	printf("\n▶ 총점은 %f, 평균은 %.2f입니다.\n\n", total, total / (STUDENTS * SUBJECTS));
	return b;
}
```
```c
// 9. 전체 학급의 과목별 평균 출력 함수 정의
void total_subject_avg(double* b, int a) // 과목별 전체학급 성적 배열, 학급수
{
	double avg;       // 과목별 총점으로 평균을 계산하여 넣을 <과목별 전체 평균 변수> 선언
	printf("\n\n===== << 전체 학급의 과목별 평균 >> =====\n\n");
	for (int j = 0; j < SUBJECTS; j++)
	{
		avg = *(b + j/8) / (a * STUDENTS);       // 과목별 전체학생의 평균 점수 = (전체학급의 과목별 점수)/(학급수X학생수)
		printf("       과목 % d의 총 평균 : % .2f\n", j, avg);
	}
}
```
