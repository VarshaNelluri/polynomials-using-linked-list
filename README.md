# polynomials-using-linked-list
#include<stdio.h>
#include<stdlib.h>
struct poly
{
        int exp,coeff;
        struct poly *next;
};
typedef struct poly node;
node *addterm(node *first,node *new)
{
        node *temp,*temp1;
        if(first==NULL)
        {
                first=new;
        }
        else if(first->exp<new->exp)
        {
                new->next=first;
                first=new;
        }
        else if(first->exp==new->exp)
        {
                first->coeff=first->coeff+new->coeff;
        }
        else
        {
                temp1=first;
                temp=first->next;
                while(temp!=NULL && temp->exp>new->exp)
                {
                        temp1=temp;
                        temp=temp->next;
                }
                if(temp==NULL)
                {
                        temp1->next=new;
                }
                else if(temp->exp==new->exp)
                {
                        temp->coeff=temp->coeff+new->coeff;
                }
                else
                {
                        new->next=temp;
                        temp1->next=new;
                }
        }
        return first;
}
node *create()
{
        node *first=NULL,*new;
        int x;
        printf("Enter 1 to continue(0 to stop):");
        scanf("%d",&x);
        while(x!=0)
        {
                new=(node *)malloc(sizeof(node));
                printf("enter exponential:\n");
                scanf("%d",&new->exp);
                printf("enter coefficient:\n");
                scanf("%d",&new->coeff);
                first=addterm(first,new);
                printf("Enter 1 to continue(0 to stop):");
                scanf("%d",&x);
        }
        return first;
}
node *add(node *p1,node *p2)
{
        node *p3=NULL,*new;
        while(p1!=NULL && p2!=NULL)
        {
                new=(node *)malloc(sizeof(node));
                new->next=NULL;
                if(p1->exp>p2->exp)
                {
                        new->exp=p1->exp;
                        new->coeff=p1->coeff;
                        p3=addterm(p3,new);
                        p1=p1->next;
                }
                else if(p1->exp<p2->exp)
                {
                        new->exp=p2->exp;
                        new->coeff=p2->coeff;
                        p3=addterm(p3,new);
                        p2=p2->next;
                }
                else
                {
                        new->exp=p2->exp;
                        new->coeff=p1->coeff+p2->coeff;
                        p3=addterm(p3,new);
                        p1=p1->next;
                        p2=p2->next;
                }
        }
        if(p1==NULL)
        {
                while(p2!=NULL)
                {
                        new=(node *)malloc(sizeof(node));
                        new->exp=p2->exp;
                        new->coeff=p2->coeff;
                        p3=addterm(p3,new);
                        p2=p2->next;
                }
        }
        if(p2==NULL)
        {
                while(p1!=NULL)
                {
                        new=(node *)malloc(sizeof(node));
                        new->exp=p1->exp;
                        new->coeff=p1->coeff;
                        p3=addterm(p3,new);
                        p1=p1->next;
                }
        }
        return p3;
}
node *sub(node *p1,node *p2)
{
        node *p3=NULL,*new;
        while(p1!=NULL && p2!=NULL)
        {
                new=(node *)malloc(sizeof(node));
                new->next=NULL;
                if(p1->exp>p2->exp)
                {
                        new->exp=p1->exp;
                        new->coeff=p1->coeff;
                        p3=addterm(p3,new);
                        p1=p1->next;
                }
                else if(p1->exp<p2->exp)
                {
                        new->exp=p2->exp;
                        new->coeff=-p2->coeff;
                        p3=addterm(p3,new);
                        p2=p2->next;
                }
                else
                {
                        new->exp=p2->exp;
                        new->coeff=p1->coeff-p2->coeff;
                        p3=addterm(p3,new);
                        p1=p1->next;
                        p2=p2->next;
                }
        }
        if(p1==NULL)
        {
                while(p2!=NULL)
                {
                        new=(node *)malloc(sizeof(node));
                        new->exp=p2->exp;
                        new->coeff=-p2->coeff;
                        p3=addterm(p3,new);
                        p2=p2->next;
                }
        }
        if(p2==NULL)
        {
                new=(node *)malloc(sizeof(node));
                while(p1!=NULL)
                {
                        new->exp=p1->exp;
                        new->coeff=p1->coeff;
                        p3=addterm(p3,new);
                        p1=p1->next;
                }
        }
        return p3;
}
node *mul(node *p1,node *p2)
{
        node *new,*p3=NULL,*temp=p2;
        while(p1!=NULL)
        {
                p2=temp;
                while(p2!=NULL)
                {
                        new=(node *)malloc(sizeof(node));
                        new->exp=p1->exp+p2->exp;
                        new->coeff=p1->coeff*p2->coeff;
                        p3=addterm(p3,new);
                        p2=p2->next;
                }
                p1=p1->next;
        }
        return p3;
}
void display(node *first)
{
        node *temp;
        if(first==NULL)
        {
                printf("No polynomial found");
        }
        else
        {
                temp=first;
                while(temp!=NULL)
                {
                        printf("%dx^%d+",temp->coeff,temp->exp);
                        temp=temp->next;
                }
                printf("\n");
        }
}
void main()
{
        node *p1,*p2,*p3;
        int ch;
        printf("enter first polynomial:\n");
        p1=create();
        printf("enter second polynomial:\n");
        p2=create();
        printf("Displaying first polynomial:\n");
        display(p1);
        printf("Displaying second polynomial:\n");
        display(p2);
        while(1)
        {
                printf("Menu Driven:\n1.Add\n2.Sub\n3.mul\n4.exit\n");
                printf("enter your choice:\n");
                scanf("%d",&ch);
                switch(ch)
                {
                        case 1:p3=add(p1,p2);
                               printf("the addition of two polynomials:\n");
                               display(p3);
                               break;
                        case 2:p3=sub(p1,p2);
                               printf("the subtraction of two polynomials:\n");
                               display(p3);
                               break;
                        case 3:p3=mul(p1,p2);
                               printf("multiplication of two polynomial:\n");
                               display(p3);
                               break;
                        case 4:exit(0);
                               break;
                        default:printf("enter correct choice\n");
                }
        }
}
