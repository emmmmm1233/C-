#include<stdio.h>            //�õ������������
#include<string.h>           //�õ��ַ�������
#include <unistd.h>
#include <stdlib.h>
#include"windows.h"          //�õ�windows����


#define MAX_TYPE 4    //�������͵���Ŀ  3
#define MAX_NUM 4	 //ÿ�����ͷ������Ŀ 3
#define MAX_GUEST 10	// ���Ŀͻ��� 9

//# define M 30                 // �û���
//# define N 20                 //������

int z0=123456;                //����Ա��ʼ�˺�
int s0=123456;               //����Ա��ʼ����


int mimaxiugai();            //�����޸ĺ���
void guanliyuan();           //����Ա��¼����
void jiemian();              //���˵�����
void fangjianxinxi();        //������Ϣ����
void yuding();               //Ԥ������
int read_printf_guest(void);                 //��ȡ����ʾ�ͻ�����
int save_guest(int count);           		//����ͻ���Ϣ����
int save_room(int type,int num);          //���淿����Ϣ����
void Outsystem();            //�˳�ϵͳ����
void chaxun();               //���ϲ�ѯ����
int read_guest();             //��ȡ�ͻ���Ϣ����
void tuiding();              //�˶�����
int read_room(void);               //��ȡ������Ϣ����
void dingdanchaxun();        //������ѯ����
void fangjian();

struct room       /*�����������Ϣ�Ľṹ��*/
{
    int type;         //��������
    int num;          //�������
    int date;		  //��ס����
    int flag;         // 0-������ס 1-��סһ�� 2-��ס����
};
struct room g_room[MAX_TYPE][MAX_NUM];    //����һ���ṹ�屣�淿�����Ϣ

struct guest       /*�����������Ϣ�Ľṹ��*/
{
    char name[30];      //�ͻ�����
    char ID[30];        //�ͻ����֤��
    char phone[15];     //�ͻ��绰
    int type;           //��������
    int number;         //�����
    int date;           //��ס����
};
struct guest guest[MAX_GUEST];  //����һ���ṹ�屣��ͻ��ĸ�����Ϣ

struct room__message   //��סʱ��д�ķ�����Ϣ
{
    int type ;
    int num;
    int date;
};



void empty_file(char * fname)
{
    FILE * fp;
    fp = fopen(fname, "w");//��ֻд��ʽ���ļ�ʱ ��ʵ���Ǵ�����һ�����ļ�(�յ�)�� ������������ļ���Ҳͬ���ᱻ����
    if(fp == NULL)
        printf("do empty file %s failed\n", fp);
    else fclose(fp);
}






void delete(char* FileName, int lineno)           //�˶� 
{
   int   Lid=0;
   int   MaxLine=0;
   FILE*   fp=NULL;
   char   Buf[256]="";
   char   tmp[20][256]={0};
   char   *p   =   Buf;

      if ((fp = fopen(FileName, "r+")) == NULL)
   {
    printf("Can't   open   file!/n");
    return;
   }

   while ((p = fgets(Buf, 256, fp)) != NULL)
   {
     Lid++;
     if (Lid == lineno)
     {
      if ((p = fgets(Buf, 256, fp)) != NULL)
      {
      strcpy(tmp[Lid], Buf);
     }
      }
    else
    {
      strcpy(tmp[Lid], Buf);
     }
   }

   MaxLine = Lid;
   rewind(fp);
   fclose(fp);
   remove(FileName);   //   ɾ��ԭ�ļ�

   if((fp = fopen(FileName, "w")) == NULL)
   {
     printf("Can't   open   file!/n");
     return;
   }

    for(Lid = 1; Lid <= MaxLine; Lid++)
    {
    fputs(tmp[Lid], fp);
    }
    fclose(fp);
}


//*���淿����Ϣ����*/
int save_room(int type,int num)
{
	//int i;
	//char c;
	FILE *fp;
	if((fp=fopen("room_data.txt","a"))==NULL)
	{
            perror("in save_room open error:");
			exit(-1);
	}

    fprintf(fp,"%d %d %d %d\n",g_room[type][num].type,g_room[type][num].num,g_room[type][num].date,g_room[type][num].flag);
    fclose(fp);
    return 0;
}


/*����ͻ���Ϣ����*/
int save_guest(int count)
{
	//int i;
	//char c;
	FILE *fp;
	if((fp=fopen("guest_data.txt","a"))==NULL)
	{
            perror("in save_guest open error:");
			exit(-1);
	}
    fprintf(fp,"%s %s %s %d %d %d\n",guest[count].name,guest[count].ID,guest[count].phone,guest[count].type,guest[count].number,guest[count].date);
    fclose(fp);
    return 0;
}


int save_mima()
{
	//int i;
	//char c;
	FILE *fp;
	if((fp=fopen("mima_data.txt","a"))==NULL)
	{
            perror("save_mima open error:");
			exit(-1);
	}

    fprintf(fp,"%d \n",s0);
    fclose(fp);
    return 0;
}


int read_mima()
{

	FILE *fp;
	if((fp=fopen("mima_data.txt","r"))==NULL)
	{
			perror("in read_mima oepn error:");
			exit(-1);
	}



    fscanf(fp,"%d\n",s0);

	fclose(fp);
	return 0;
}

/*��ȡ��������ͻ���Ϣ����*/
int read_printf_guest(void)
{
	int i,j;
	FILE *fp;
	if((fp=fopen("guest_data.txt","r"))==NULL)

	{
			printf("cannot open file\n");
			exit(-1);
	}
	printf("�ͻ�����\t���֤��\t�绰����\t��������\t�����\t��ס����\n");

	for(i=1;i<MAX_GUEST;i++)
	{
        //	fread(&room[i],sizeof(struct room),1,fp);
		//	fscanf(fp,"%d %d %d %d\n",&room[i].number,&room[i].type,&room[i].date,&room[i].flag);
		fscanf(fp,"%s %s %s %d %d %d\n",guest[i].name,guest[i].ID,guest[i].phone,&guest[i].type,&guest[i].number,&guest[i].date);
		if(guest[i].number != 0 && guest[i].date != 0)
        	printf("%s\t %s\t %s\t %d\t \t%d\t     %d\n",guest[i].name,guest[i].ID,guest[i].phone,guest[i].type,guest[i].number,guest[i].date);

	}
	fclose(fp);
	return 0;
}

/*��ȡ�ͻ���Ϣ����*/
int read_guest()
{
	int i;
	FILE *fp;
	if((fp=fopen("guest_data.txt","r"))==NULL)
	{
			perror("in read_guest oepn error:");
			exit(-1);
	}

	for(i=1;i<MAX_GUEST;i++)
	{
		fscanf(fp,"%s %s %s %d %d %d\n",guest[i].name,guest[i].ID,guest[i].phone,&guest[i].type,&guest[i].number,&guest[i].date);
	}
	fclose(fp);
	return 0;
}

/*��ȡ������Ϣ����*/
int read_room(void)
{
	int i,j;
	int type = 0,num = 0,date = 0,flag = 0;
    FILE *fp;
	if((fp=fopen("room_data.txt","r"))==NULL)
	{
            perror("in read_room open error:");
			exit(-1);
	}

	for(i = 1;i<MAX_TYPE;i++)
	{
        for(j = 1;j<MAX_NUM;j++)
        {
            fscanf(fp,"%d %d %d %d\n",&type,&num,&date,&flag);
            if(type != i && num != j)               //����������ķ������ͺͷ���ŵ�ǰ
                                                    //�����Ŀ�귿�䲻һ��ʱҪ��������
            {
                g_room[type][num].type = type;
                g_room[type][num].num = num;
                g_room[type][num].date = date;
                g_room[type][num].flag= flag;
            }
            else if(type != i )
            {
                g_room[type][j].type = type;
                g_room[type][j].num = num;
                g_room[type][j].date = date;
                g_room[type][j].flag = flag;
            }
            else if(num != j)
            {
                g_room[i][num].type = type;
                g_room[i][num].num = num;
                g_room[i][num].date = date;
                g_room[i][num].flag = flag;
            }
            else if(type == i && num == j)
            {
                g_room[i][j].type = type;
                g_room[i][j].num = num;
                g_room[i][j].date = date;
                g_room[i][j].flag = flag;
            }
        }


	}
    fclose(fp);
    return 0;
}

//*******************************************����������****************************************************

int main()
{
	unsigned int z;
	char out[] = "\t\t\t��ӭʹ�þƵ귿��Ԥ��ϵͳ\n\n";
    system("color 3F");           //��������ʾ������ɫ�޸ĵ�
	printf("���******************************���******************************���\n\n");
	for( z=0; z<strlen(out); z++ )
	{
		printf("%c",out[z]);  //��̬����ַ���
		Sleep(30);//˯��30���롣
	}
	printf("���******************************���******************************���\n");
    printf("\n");
	printf("\n");
    printf("\t��ϵͳ����ߣ�����Դ����ΰ�ס���־쿡�����浡���������*��ɽ��\n\n");
	printf("\t\t\t���c���Կγ���ơ��\n");
	Sleep(3000); //�ǵȴ�ʱ��
    jiemian();  //���溯��
	return 0;
}
//*******************************************����****************************************************
void jiemian()
{
      system("cls");
      system("color 74");
	  int a;
	  printf("\t-----------------------------------------------------------\n");
	  printf("\t���************* �ﻶӭ�������湤ҵ�Ƶ�� *************���\n");
	  printf("\t-----------------------------------------------------------\n");
	  printf("                   |&        1.������Ϣ        &|          \n");
  	  printf("                   |&        2.Ԥ��            &|          \n");
      printf("                   |&        3.�˶�            &|          \n");
	  printf("                   |&        4.������ѯ        &|          \n");
 	  printf("                   |&        5.����Ա��½      &|          \n");
	  printf("                   |&        6.�˳�ϵͳ        &|          \n\n");

	  printf("              ������1-3��enter��ѡ��������ķ���\n ");
Loop_1:                                                              //���������ص�ѭ����ʶ��
	scanf("%d",&a);
	if(a!=1&&a!=2&&a!=3&&a!=4&&a!=5&&a!=6)
	{
		  printf("����ȷ�����Ӧ��ţ�����\n");
		   goto Loop_1;                                              //����ǰ�涨���˵ı�ʶ��
	}
	switch(a)
    {
		  case 1:
			  fangjianxinxi();break;
		  case 2:
			 fangjian();break;
          case 3:
			 tuiding();break;
		  case 4:
			 dingdanchaxun();break;
		  case 5:
			 guanliyuan();break;
		  case 6:
			 Outsystem();break;

	}
}
//*******************************************������Ϣ����***********************************************
void fangjianxinxi()
{
    	int j;
    system("cls");//����
	system("color 3E");//Ԥ����Ļ������ɫ
	printf("���========================================================================���\n");
	printf("                   ��ӭ�������湤ҵ�Ƶ귿����Ϣ�������\n");
    printf("                   ====================================\n\n");
	printf("���*********************���������************************���\n");
	printf("                         ��  ������Ϣ  ��\n");
	printf("���*********************���������************************���\n");
	printf("          ||     ��ͨ���˷���  ���˴�  1-2¥   ��200/��     ||\n");
    printf("          ||     ��׼˫�˷���  ˫�˴�  3-4¥   ��300/��     ||\n");
	printf("          ||     �����󴲷���   �� ��   5 ¥   ��500/��     ||\n");
	printf("\n  �к������ķ������Ƿ�Ҫ�Ǽ���ס:\n");
	printf("                            1������Ԥ������\n");
    printf("                            2��������һ��\n");
	printf("                            3���˳�ϵͳ\n");
    printf("����������1-3ѡ��\n");
Loop_7:

	scanf("%d",&j);
	if (j!=1&&j!=2&&j!=3)
	{
		printf("\n��������!  ����������,лл��\n\n\n");
		goto Loop_7;

	}
	else
	{
		switch(j)
		{
		case 1:
			 fangjian();break;
		case 2:
			 jiemian();break;
		case 3:
			 Outsystem();break;
		}
	}
}
//******************************************* �Ǽ���ס����***********************************************



void fangjian(void)
{
    struct room__message room;
    static int count = 1;
    int num_day = 0;
    int sure = 0;
    char c;
    read_room();
	system("cls");//����
	system("color 74");//Ԥ�������ɫ
    printf(" ���*******************����Ϊ��������,��ͻ��������*******************���\n");
    printf("         1����Я����ס�����֤,ƾ������Ԥ���ֻ���ֱ�Ӱ�����ס\n");
    printf("         2����סʱ��:�뵱��14:00֮����ס���ڴ���12:00֮ǰ�˷�;\n");
    printf("            ������ǰ��ס����ʱ�˷�,����ѯ�̼�\n");
    printf("         3����ס��ҪѺ��,�����ǰ̨Ϊ׼\n");
    printf(" ���******************************���******************************���\n");

    printf("\n\n********************************�Ǽ���ס********************************\n\n");
    printf("\n��������������ס�ķ�������    1�������󴲷�    2����׼˫�˷�    3����ͨ���˷�\n");
    scanf("%d",&room.type);
    while(room.type!=1&&room.type!=2&&room.type!=3)
    {
        printf("����������,����������1��2��3,���ٴ�����:\n");
        scanf("%d",&room.type);
    }

    printf("\n\n����������ס�ķ����(1-2-3):\n");
    scanf("%d",&room.num);
    while(room.num!=1&&room.num!=2&&room.num!=3)
    {
        printf("����������,����������1��2��3,���ٴ�����:\n");
        scanf("%d",&room.num);
    }

    printf("\n\n��������������ס�����ڣ�\n");
    scanf("%d",&room.date);
    while(room.date<1||room.date>31)
    {
        printf("����������,����������:\n");
        scanf("%d",&room.date);
    }

    printf("\n\n��������Ԥ��������:(���ֻ��Ԥ������):\n");
    scanf("%d",&num_day);
    while(num_day>2||num_day<=0)
    {
        printf("����������,����������(���ֻ��Ԥ������):\n");
        scanf("%d",&num_day);
    }
    if(num_day ==  1)
    {
        if((g_room[room.type][room.num].flag == 1)  && (g_room[room.type][room.num].date == room.date) || ((g_room[room.type][room.num].flag == 2) && (g_room[room.type][room.num].date + 1 == room.date)) )
        {
            printf("��Ǹ��������ҪԤ���ķ���������\n\n");
            Sleep(3000); //�ǵȴ�ʱ��
            printf("�밴����������½���Ԥ��:\n");
    		scanf("%c",&c);
    		scanf("%c",&c);
            fangjian();
          	return;
        }
        else
        {
            //�Ǽ�������Ϣ��д������Ϣ
            if(room.type == 1)
                printf("\n��ѡ����Ǻ����󴲷�, ��500/��,���ķ���%d!\n",room.num);
            else if(room.type == 2)
                printf("\n��ѡ����Ǳ�׼˫�˷�,��300/��,���ķ���%d!\n",room.num);
            else if(room.type == 3)
                printf("\n��ѡ�������ͨ���˷�,��200/��,���ķ���%d!\n",room.num);

            printf("\n\n����������������\n");
            scanf("%s",guest[count].name);
            while(strlen(guest[count].name)<0)
             {
                 printf("\n\n����������������,������������:\n");
                 scanf("%s",guest[count].name);
             }

            printf("\n\n�������������֤���룺\n");
            scanf("%s",guest[count].ID);
            while((strlen(guest[count].ID)) != 18)
            {
                printf("\n\n�������֤������������,������������:\n");
                scanf("%s",guest[count].ID);
            }

            printf("\n\n�����������ֻ����룺\n");
            scanf("%s",guest[count].phone);
            while((strlen(guest[count].phone)) != 11)
            {
                printf("\n\n�����ֻ�������������,������������:\n");
                scanf("%s",guest[count].phone);
            }

            printf("\n\n���Ƿ�ȷ��������Ϣ:(1-��,2-��):\n");
            scanf("%d",&sure);
            while(sure<1 || sure>2)
            {
                printf("\n\n����ѡ������,������������:\n");
                scanf("%d",&sure);
            }
             if(sure == 1)
             {
                 printf("\n\n���ѵǼ���ס�ɹ�,ף���ڱ���ס�����!\n\n");
                 Sleep(2000);
            }
             else
             {
                 printf("\n\n�����µǼ�\n");
                 Sleep(1000);
                 fangjian();
                 return;
             }
        }
        guest[count].number = room.num;
	    guest[count].date = room.date;
	    guest[count].type = room.type;
	    save_guest(count);      //���浱ǰ�ͻ�����Ϣ
	    count++;
	    if(count == 10)
	    {
	    	count = 1;
		}

	    g_room[room.type][room.num].type = room.type;
	    g_room[room.type][room.num].num = room.num;
	    g_room[room.type][room.num].date = room.date;
	    g_room[room.type][room.num].flag = 1;
	    save_room(room.type,room.num);
    }
    else if(num_day == 2)  //�������Ԥ������
    {

    	if((g_room[room.type][room.num].date == room.date) && (g_room[room.type][room.num].flag == 2) || (g_room[room.type][room.num].date == room.date) && (g_room[room.type][room.num].flag == 1))
        {
            printf("��Ǹ��������ҪԤ���ķ���������\n");
            Sleep(3000); //�ǵȴ�ʱ��
            printf("�밴����������½���Ԥ��:\n");
    		scanf("%c",&c);
    		scanf("%c",&c);
            fangjian();
          	return;
        }
        else
        {
            //�Ǽ�������Ϣ��д������Ϣ
            if(room.type == 1)
                printf("\n��ѡ����Ǻ����󴲷�,��500/��,���ķ���%d!\n",room.num);
            else if(room.type == 2)
                printf("\n��ѡ����Ǳ�׼˫�˷�,��300/��,���ķ���%d!\n",room.num);
            else if(room.type == 3)
                printf("\n��ѡ�������ͨ���˷�,��200/��,���ķ���%d!\n",room.num);

            printf("\n\n����������������\n");
            scanf("%s",guest[count].name);
            while(strlen(guest[count].name)<0)
             {
                 printf("\n\n����������������,������������:\n");
                 scanf("%s",guest[count].name);
             }

            printf("\n\n�������������֤���룺\n");
            scanf("%s",guest[count].ID);
            while((strlen(guest[count].ID)) != 18)
            {
                printf("\n\n�������֤������������,������������:\n");
                scanf("%s",guest[count].ID);
            }

            printf("\n\n�����������ֻ����룺\n");
            scanf("%s",guest[count].phone);
            while((strlen(guest[count].phone)) != 11)
            {
                printf("\n\n�����ֻ�������������,������������:\n");
                scanf("%s",guest[count].phone);
            }

            printf("\n\n���Ƿ�ȷ��������Ϣ:(1-��,2-��):\n");
            scanf("%d",&sure);
            while(sure<1 || sure>2)
            {
                printf("\n\n����ѡ����������,������������:\n");
                scanf("%d",&sure);
            }
             if(sure == 1)
             {
                 printf("\n\n����Ԥ���ɹ�,ף���ڱ���ס�����!\n\n");
                 Sleep(2000);
            }
             else
             {
                 printf("\n\n�����µǼ�\n");
                 Sleep(1000);
                 fangjian();
                 return;
             }
        }
        guest[count].number = room.num;
	    guest[count].date = room.date;
	    guest[count].type = room.type;
	    save_guest(count);      //���浱ǰ�ͻ�����Ϣ
	    count++;
	    if(count == 10)
	    {
	    	count = 1;
		}

	    g_room[room.type][room.num].type = room.type;
	    g_room[room.type][room.num].num = room.num;
	    g_room[room.type][room.num].date = room.date;
	    g_room[room.type][room.num].flag = 2;
	    save_room(room.type,room.num);
	}

    printf("�����������������:");
    scanf("%c",&c);
    scanf("%c",&c);
    jiemian();
}







//******************************************* �û�������Ϣ��ѯ����***********************************************
void dingdanchaxun()
{
	read_guest();
	static label = 0;
	int i,flag=1;  //flagҲ����������λ��־
	int a;
	char id[19];
	printf("�������������֤��:��18λ��\n");
	scanf("%s",id);
	for(i=1;i<MAX_GUEST;i++)
	{                                             // �����еļ�¼�в���
		if(strcmp(id,guest[i].ID) == 0)          //�ҵ���¼�������Ϣ
		{
            printf("\n\t\t\t���ѳɹ������û��������桤����������\n\n");
            Sleep(1000);
			printf("\n\n���******************************���******************************���\n");
			printf("���             ���         ���Ķ�����Ϣ         ���             ���\n");
			printf("���******************************���******************************���\n\n");
		    printf(" ����        : %s\n",guest[i].name);
		    printf("���֤��     : %s\n",guest[i].ID);
			printf("�绰����     : %s\n\n",guest[i].phone);
			if(guest[i].type == 3)
				{
					printf("��������     : ��ͨ���˷�\t\t\n");
					printf("�����       : %d ��\n",guest[i].number);
					a=200;
				}

			if(guest[i].type == 2)
			 	{
					printf("��������     : ��׼˫�˴�\t\t\n");
					printf("�����       : %d ��\n",guest[i].number);
					a=300;
				}

			if(guest[i].type == 1)
				{
					printf("��������     : �����󴲷�\t\t\n");
					printf("�����       : %d ��\n",guest[i].number);
					a=500;
				}

		    flag=0;//�����жϹ˿��������Ϣ��ѯ�Ƿ��д�
		    printf("����Ԥ������ ��%d ��\t\t",guest[i].date);
		    printf("��Ӧ������   ��%d Ԫ \n" ,guest[i].date*a);
			printf("\n�����Խ������в��� ��\n  \t\t\t1-���ؽ���\n  \t\t\t\n");
			scanf ("%d",&i);
			while(i!=1&&i!=2)
		 {  printf("��������������ѡ��\n");
		    scanf("%d",&i);
		 }
	        switch(i)
		    {
			case 1: jiemian();break;
	//	    case 2:tuiding();break;
		    }

        }

	//	else
	//		continue;  //��������
    }
	if(flag)
	{
		label++;
		if(label == 3)
		{
			printf("�������δ��󣬷���������:");
			Sleep(3000);
			jiemian();
			return;
		}
		printf("û�в�ѯ������Ҫ�ļ�¼!!\n");
		printf("����������:\n");
		Sleep(1000);
		jiemian();
		return;
	}

	Sleep(3000);
	jiemian();
}

//*******************************************����Ա��¼����***********************************************
void guanliyuan()
{
	system("cls");//����
	system("color 74");//���ý�����ɫ
	int a,c;
	int s;
	int flag = 0;
	fflush(stdin);
	system("cls");
	printf("\n");
	printf("             ��********************************��\n");
	printf("                   �� ��ӭ�������Աϵͳ ��\n");
	printf("             ��********************************��\n");
	printf("\n");
	read_mima();
Loop_22:
	printf("���������Ա�˺ţ�\n");
	scanf("%d",&c);
	printf("���������Ա���룺\n");

    scanf("%d",&s);                                           // �����˺����� 

    if((s-s0)== 0 && (c-z0)== 0)                             // ����˺����붼������ȷ
	{
		char message_hello[] = "\t\t\t����Ա,���!��ѡ������Ҫ�ķ������ͣ�\n\n";
		unsigned int i;
		for(i = 0;i < strlen(message_hello); i++)              //��̬����ַ���
		{
			printf("%c",message_hello[i]);
			Sleep(50);
		}
		Sleep(100);
		printf("1�������ѯ             2�������޸�            3���˳�ϵͳ\n\n");
		printf("��ѡ��:\n");
		scanf("%d",&a);
		if(a!=1&&a!=2&&a!=3)
		{
			printf("�����������������1��2��3�����ٴ����룺\n");
			scanf("%d",&a);

		}
			switch(a)
         {
		  case 1:
			  chaxun();break;
		  case 2:
			  mimaxiugai();break;
		  case 3:
			  Outsystem();break;
	     }
    }
    else
	{
		flag ++;
		if(flag == 3)
		{
			printf("������󳬹����Σ�ϵͳ�Զ��˳�.....");
			Sleep(2000);
			Outsystem();
		}
		printf("�����������������:\n\n");
		goto Loop_22;
	}

}
//*******************************************�����ѯ����***********************************************
void chaxun()
{
	system("color 74");
	system("cls");
	int a;
	int b;
	int c;
	int e;
	int f;
	int i;

	printf("��ѡ������Ҫ�Ĳ�ѯ���ͣ�\n     1����ʷԤ����Ϣ     2�������Ų�ѯ     3�������ڲ�ѯ\n");
	scanf("%d",&a);
	while(a!=1&&a!=2&&a!=3)
	{
		printf("���������룺\n");
		scanf("%d",&a);
	}
	if(a==1)
	{
		system("cls");
		read_printf_guest();
		printf("\n1��������һ��         2���������˵�         3���˳�ϵͳ\n");
		scanf("%d",&b);
		if(b!=1&&b!=2&&b!=3)
		{
		    printf("����������0~3�����������룺\n");
		    scanf("%d",&b);
		}
		switch(b)
		{
		case 1:
			chaxun();
		case 2:
			jiemian();
		case 3:
			Outsystem();
		}
	}
	if(a==2)
	{
		system("cls");
		printf("����������Ҫ��ѯ�ķ���ţ�\n");
		scanf("%d",&c);
		while(c!=1&&c!=2&&c!=3)
		{
		    printf("���������룺\n");
		    scanf("%d",&c);
		}
        printf("�ͻ�����\t���֤��\t�绰����\t��������\t�����\t��ס����\n");
		read_guest();
	    for(i=0;i<MAX_GUEST;i++)
		{
			if(guest[i].number==c)
		    printf("%s\t %s\t %s\t %d\t %d\t %d\n",guest[i].name,guest[i].ID,guest[i].phone,guest[i].type,guest[i].number,guest[i].date);
		}
			printf("\n\t1��������һ��     2���������˵�     3���˳�ϵͳ\n");
		scanf("%d",&b);
		while(b!=1&&b!=2&&b!=3)
		{
		    printf("���������룺\n");
		    scanf("%d",&b);
		}
		switch(b)
		{
		case 1:
			chaxun();
		case 2:
			jiemian();
		case 3:
			Outsystem();
		}
	}
	if(a==3)
	{
		system("cls");
		printf("����������Ҫ��ѯ�����ڣ�\n");
		scanf("%d",&e);
        printf("�ͻ�����\t���֤��\t�绰����\t��������\t�����\t��ס����\n");
	   int read_guest();
		for( i=0;i<MAX_GUEST;i++)
		{
			if(guest[i].date==e)
			printf("%s\t %s\t %s\t %d\t %d\t %d\n",guest[i].name,guest[i].ID,guest[i].phone,guest[i].type,guest[i].number,guest[i].date);
		}
			printf("\n1��������һ�㣻2���������˵���3���˳�ϵͳ\n");
		scanf("%d",&b);
		while(b!=1&&b!=2&&b!=3)
		{
		    printf("���������룺\n");
		    scanf("%d",&b);
		}
		switch(b)
		{
		case 1:
			chaxun();
		case 2:
			jiemian();
		case 3:
			Outsystem();
		}
	}

}
//*******************************************�����뺯��***********************************************
int mimaxiugai()
{
    system("cls");

    int x;
	printf("�Ƿ���Ĺ���Ա��½���룿\n");
	printf("��ѡ��\n\n");
	printf("\t\t       ********************************\n");
	printf("\t\t       ||      1����������           ||\n");
    printf("\t\t       ||      2�����ع���Ա����     ||\n");
Loop_m:
	scanf("%d",&x);
	if (x == 1)
	{
		int *p;
        int m;
		p=&s0;
		printf("�������µ����룺");
		scanf("%d",&m);
		*p = m;
		printf("�޸�����ɹ���");
		empty_file("mima_data.txt");
		save_mima();
		Sleep(1000);
	    guanliyuan();
		return s0;    //���ص����޸ĺ�Ĺ���Ա��½����
	}
  else if(x == 2)
  {
	  guanliyuan();

  }
  else
  {
	  printf("������������������������룺\n");
      Sleep(1000);
	  goto Loop_m;                   //�����������д�ʱ����ת��־
  }
}
//*******************************************�˳����溯��***********************************************
void Outsystem()
{
	fflush(stdin);
	system("cls");
	printf("\n");
	printf("                    ��******************************��\n");
	printf("                              �� ��лʹ�� ��\n");
	printf("                    ��******************************��\n");
	printf("\n");
	int i;
	printf(" ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^  \n");
	printf("                         ^-^ ^-^ ^-^ ^-^ ^-^ ^-^   \n");
	printf(" ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^  \n");
	char message_out[] = "\n\t���湤ҵ�Ƶ귿��Ԥ��ϵͳ,�ǳ���л���ʹ�ã���ӭ������\n\n\t\t\t\n\n\n\n";
	printf(" ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^  \n");
	printf("                         ^-^ ^-^ ^-^ ^-^ ^-^ ^-^   \n");
	printf(" ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^ ^-^  \n");
	for( i = 0; i < strlen(message_out ); i++ )
	{
		printf("%c",message_out[i]);
		Sleep(30);//˯��30���롣
	}
	printf("\n");
	exit(0);
}


void tuiding()

{

    char id[19];
    int i,r;



      printf("����������Ҫ�˶�����ݺţ�\n");
      scanf("%s",id);

	    read_guest();
		for( i=1;i<MAX_GUEST;i++)
		{

            if(strcmp(id,guest[i].ID) == 0)
           {


			printf(" ������%s\n ���֤���룺%s\n �ֻ����룺%s\n ���ţ�%d��\n Ԥ�����ڣ�%d\n\n",guest[i].name,guest[i].ID,guest[i].phone,guest[i].number,guest[i].date);
			printf("YES(����1) or NO(����2)\n");
			 scanf("%d",&r);
			 if(r==1)
             {
             delete("guest_data.txt",i);
            delete("room_data.txt",i);
             printf("�˶��ɹ�");
             Sleep(2000);

              jiemian();
             }
             if(r==2)
             {
               jiemian();
             }
           }
		}





}
