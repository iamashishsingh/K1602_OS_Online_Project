# K1602_OS_Online_Project

# include<stdio.h>
#include<pthread.h>
#include<semaphore.h>
#include<time.h>

struct tm * timeinfo;
int stud_size=0,tech_size=0;

struct que
{
    int priority;
    int bt_time;
    int r_time;
    int ar_time;
    int turn;
}
struct que stud_que[10],tech_que[10];

void gettime()
{
    time_t rawtime;
    time ( &rawtime );
    timeinfo = localtime ( &rawtime );

}

int check=0;
void check_time()
{
    gettime();
    if(timeinfo->tm_hour>=10 && timeinfo->tm_hour<12)
    {
        check=1;
    }
}

int main()
{
  
}
