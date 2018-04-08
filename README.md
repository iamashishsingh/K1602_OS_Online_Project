# K1602_OS_Online_Project

/*Q5. Sudesh Sharma is a Linux expert who wants to have an online system where he can
handle student queries. Since there can be multiple requests at any time he wishes to dedicate
a fixed amount of time to every request so that everyone gets a fair share of his time. He will
log into the system from 10am to 12am only. He wants to have separate requests queues for
students and faculty. Implement a strategy for the same. The summary at the end of the
session should include the total time he spent on handling queries and average query time.
*/

#include<stdio.h>
#include<semaphore.h>
#include<string.h>
#include<time.h>
#include<pthread.h>
#include<dos.h>


struct tm * timeinfo;
int check=0;
int time_quantum=4;
char character;
int student_size=0,teacher_size=0;
int total_size=0;
int x=0,y=0;

struct query
{
    int priority;
    int burst_time;
    int arrival_time;
    int remaining_time;
    int turn;
    char name[20];
};

struct query student_query[10],teacher_query[10];
struct query *temp;
struct query *temp2;
int student_process[10];
int teacher_process[10];

void gettime();  // function to get time
void check_time(); // function to check time
void input(); // for input of student;

void print_data(struct query a[],int size)   // for  printing data;
{
    for(int i=size-1;i<size;i++)
    {
        printf("\n%s",a[i].name);
        printf("\n%d arrival time\n",a[i].arrival_time);
        printf("%d burst time \n",a[i].burst_time);
    }

}

void status()
{
    printf(" \n No Query Present Please Enter a Query\n");
}

void summary(struct query a[],int size,struct query b[],int size1)
{
    float average_time;
    int total_time[10];
    int sum=0;

    if(total_size>0)
    {
        printf("Total Number of Queries -> %d",total_size);
        for(int i=0;i<total_size;i++)
        {
            total_time[i] = a[i].burst_time+ b[i].burst_time;
        }
        for(int i=0;i<total_size;i++)
        {
            sum = sum + total_time[i];
        }

        printf("\n total time taken %d",sum);

       average_time = sum/total_size;

        printf("\n average time taken %f\n",average_time);

    }
    else
    {
        printf("No Queries Yet\n");

    }
}

void check_name()
{
    char enter_name[10];
    printf("Enter Your name to check status : ");
    scanf("%s",&enter_name);
    if(temp2->name==enter_name)
    {
        printf("%d arrival time ",temp2->arrival_time);
        printf("%d burst time ",temp2->burst_time);
    }
}

void to_check(int a[])
{
    for(int i=0;i<10;i++)
    {
        if(a[0]==1)
        {
            printf("Wait till your name is called \n");
            break;
        }
        if(a[0]==2)
        {
             printf("Wait till your name is called \n");
             break;
        }
    }
}

void *process(struct query *temp)
{
     for(int i=total_size;i<=total_size;i++)
     {

    temp->remaining_time = temp->burst_time-time_quantum;
    if(temp->remaining_time<0)
    {
        Sleep(temp->burst_time*1000);
        printf("%s Your Query has been Executed\n",temp->name);
        break;
    }
    if(temp->remaining_time>0)
    {
        printf("%s Please Wait for Your Next Turn\n",temp->name);
        if(temp->remaining_time<=time_quantum)
        {
            temp->turn++;
            if(temp->turn==i)
                     printf("%s Your Query has been Executed\n",temp->name);
        }
    }

     }
    pthread_exit(NULL);
}


int main()
{
    pthread_t p1;

    int LEN=150;
   char buf[LEN];
   time_t curtime;
   struct tm *loc_time;
   curtime = time (NULL);
   loc_time = localtime (&curtime);
   strftime (buf, LEN, "Time is %I:%M %p.\n", loc_time);
   fputs (buf, stdout);

  if(loc_time>="10:00 AM" || loc_time<="12:00 AM")
    check=1;
  else
    check=0;

    int choice;
   // check=1;

    if(check==1)
    {
        printf(":Logging into querying system:\n\n");
    do
    {
    printf("\n:Enter your Choice:\n");
    printf(":Press 1 if you are a Student:\n");
    printf(":Press 2 if you are a Teacher:\n");
    printf(":Press 3 to know your details\n");
    printf(":press 4 to check status\n");
    printf(":press 5 for Summary\n");
    scanf("%d",&choice);

        switch(choice)
        {

        case 1:
            system("CLS");
            character ='s';
            input();
            total_size++;
             temp=&student_query[student_size];
             break;
        case 2:
             system("CLS");
             character ='t';
             input();
             total_size++;
            temp=&teacher_query[teacher_size];
            break;
        case 3:
             system("CLS");
            if(character=='s')
            {

                  temp2=&student_query[student_size];
                  check_name();
                  print_data(student_query,student_size);
            }
            if(character=='t')
            {
                 temp2=&teacher_query[teacher_size];
                 check_name();
                 print_data(teacher_query,teacher_size);
            }
            if(character!='s' && character!='t')
            {
                status();
            }
            break;
        case 4:
            system("CLS");
            if(character=='s')
            {
                temp=&student_query[student_size];
                for(int i=0;i<total_size;i++)
                {
                pthread_create(&p1,NULL,process,(void *)&student_query[i]);
                pthread_join(p1,NULL);
                }
            }

                 if(character=='t')
            {
                  temp=&teacher_query[teacher_size];
                for(int i=0;i<total_size;i++)
                {
                  pthread_create(&p1,NULL,process,(void *)&teacher_query[i]);
                  pthread_join(p1,NULL);
                }
            }
            if(character!='s' && character!='t')
            {
                status();
            }
            break;

        case 5:
             system("CLS");
             summary(student_query,student_size,teacher_query,teacher_size);
             break;
    }

    }while(choice!=0);
 }
 else
 {
    printf("\n Cannot Submit Query At this time of the day\n");
 }
}


void gettime()
{
    time_t rawtime;
    time ( &rawtime );
    timeinfo = localtime ( &rawtime );
}

void check_time()
{
    gettime();
    if(timeinfo->tm_hour>=10 && timeinfo->tm_hour<12)
    {
        check=1;
    }
}

int ar_time=0;
int bur_time;
int size;
char p_name[20];

void input()
{
    if(character=='s')
    {
        printf("\n:Hello Student:\n");
        printf("Enter your name: ");
        scanf("%s",&p_name);
    //    printf("Enter Your Arrival Time: ");
      //  scanf("%d",&ar_time);
        printf("Enter Your Burst Time: ");
        scanf("%d",&bur_time);

        temp=&student_query[student_size];
        strcpy(temp->name,p_name);
        student_query[student_size].arrival_time = ar_time;
        ar_time++;
        student_query[student_size].burst_time = bur_time;
        student_process[student_size]=1;

        student_size++;
        temp->priority=2;
    }
    if(character=='t')
    {
        printf("\n:Welcome:\n");
        printf("Enter your name: ");
        scanf("%s",p_name);
        printf("Enter Your Burst Time: ");
        scanf("%d",&bur_time);

          temp = &teacher_query[teacher_size];
          strcpy(temp->name,p_name);
          teacher_query[teacher_size].arrival_time = ar_time;

        ar_time++;

             teacher_query[teacher_size].burst_time = bur_time;
        teacher_process[teacher_size]=2;

        teacher_size++;
        temp->priority=1;
        }

    }




