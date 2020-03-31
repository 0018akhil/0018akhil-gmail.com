#include<stdio.h>
#include <unistd.h>
	#include <pthread.h>
	#include <sched.h>
	#include <semaphore.h>
	#define MAX 5
	#define NORTH 0
	#define EAST 1
	#define WEST 2
	#define SOUTH 3
	
	struct car
	{int indir;
	int outdir;
	int no;
	};
	sem_t mutex,carLock;
	int conflictcount = 0;
	char* getdirection(int i)
	{char c[5];
	switch(i)
	{case 0:return "NORTH";
	case 1:return "EAST";
	case 2:return "WEST";
	case 3:return "SOUTH";
	}
	}
	void* Leave(struct car* arg)
	{sem_wait(&carLock);
	printf("\n\tcar %dleaving to %s",arg->no,getdirection(arg->outdir));
	sem_post (&carLock);
	return 0;
	}
	void* Enter(struct car* arg)
	{int indir = arg->indir;
	int outdir = arg->outdir;
	printf("\ncar %d is moving from %s to %s",arg->no,getdirection(indir),getdirection(outdir));
	sem_wait(&mutex);
	if((indir==0 || indir==1)&& outdir==2)
	{conflictcount++;}
	if(conflictcount==1){sem_wait(&carLock);}
	sem_post(&mutex);
	Leave(arg);
	printf("\ncar %d is moved from %s to %s",arg->no,getdirection(indir),getdirection(outdir));
	sem_wait(&mutex);
	if(indir==0 ||indir==1 ){conflictcount--;}
	if(conflictcount==0){sem_post(&carLock);}
	sem_post(&mutex);
	return 0;
	}
	

	int main()
	{pthread_t carthread[MAX];
	int i;
	struct car c[5];
	sem_init(&mutex,0,1);
	printf("Enter the directions:[NORTH 0,EAST 1,WEST 2,SOUTH 3]\n");
	for(i=0; i<MAX; i++)
	{printf("enter indir and outdir of car%d:",i+1);
	scanf("%d",&c[i].indir);scanf("%d",&c[i].outdir);
	c[i].no=i+1;
	}
	for(i=0; i<MAX; i++)
	{pthread_create(&carthread[i],0,Enter,&c[i]);
	// pthread_join(carthread[i],0);
	}
	for(i=0; i<MAX; i++)
	{pthread_join(carthread[i],0);}
	sem_destroy (&mutex);
	return 0;
	}
