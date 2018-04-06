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
    char person_name[15];
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

void print_data(struct que a[],int size)
{

for(int i=0;i<size;i++)
    {
        printf("%d %d %d %s\n",a[i].priority,a[i].ar_time,a[i].bt_time,a[i].person_name);
    }
}

int min_av_student=1000,stud_loc,min_turn=100;
void pro_min_student()
{
    for(int i=0;i<stud_size;i++)
    {
        if(min_av_student>stud_que[i].ar_time && min_turn>=stud_que[i].turn)
        {
                min_av_student=stud_que[i].ar_time;
                stud_loc=i;
        }

    }
}

int min_av_teacher=1000,tech_loc;
void pro_min_teacher()
{
    for(int i=0;i<tech_size;i++)
    {
        if(min_av_teacher>tech_que[i].ar_time)
        {
                min_av_teacher=tech_que[i].ar_time;
                tech_loc=i;
                printf("yoo\n");
        }

    }
}
int quantom=20;

int main()
{
  
}
