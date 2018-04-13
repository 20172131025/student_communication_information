# student_communication_information
综合案例——小型学生通讯录管理系统
//avl.cpp

#include "AVLTree.h"

AVLTree MakeEmpty(AVLTree T)

{

    if(T!=NULL)

    {

        MakeEmpty(T->left);

        MakeEmpty(T->right);

        delete T;

    }

    return NULL;

}

Position Find(string x,AVLTree T)

{

    if(T!=NULL)

    {

        if(T->data->name==x)

            return T;

        else if(T->data->name<x)

            Find(x,T->right);

        else Find(x,T->left);

    }

    else

    {

        cout<<"NOT FOUND"<<endl;

        return NULL;

    }

}

void display(Position p)

{

    cout<<"Name:"<<p->data->name<<endl;

    cout<<"Number:"<<p->data->phonenumber<<endl;

    cout<<"Address:"<<p->data->address<<endl;

    return;

}

int Max(Position p1,Position p2)

{

    if(p1->data->name>p2->data->name)

        return 1;

    else if(p1->data->name<p2->data->name)

        return -1;

    else return 0;

}

int MaxHeight(Position p1,Position p2)

{

    if(p1->height==p2->height||p1->height>p2->height) return p1->height;

    else return p2->height;

}

AVLTree Insert(string name,string number,string address,AVLTree T)

{

    if(T==NULL)

    {

        try

        {

            Position T=new AVLNode;

            T->data->name=name;

            T->data->phonenumber=number;

            T->data->address=address;

            T->height=0;

            T->right=NULL;

            T->left=NULL;

        }

        catch(const bad_alloc& e)

        {

            cerr<<"Out of Space."<<endl;

        }

    }

    else if(name==T->data->name) cerr<<"The man was in the address book."<<endl;

    else if(name>T->data->name) Insert(name,number,address,T->right);

    else Insert(name,number,address,T->left);

    return T;

}

static Position SRL(Position k2)//Single rotate with left

{

    Position k1=k2->left;

    k2->left=k1->right;

    k1->right=k2;

    k2->height=MaxHeight(k2->left,k2->right)+1;

    k1->height=MaxHeight(k1->left,k2)+1;

    return k1;

}

static Position SRR(Position k1)//Single rotate with right

{

    Position k2=k1->right;

    k1->right=k2->left;

    k2->left=k1;

    k1->height=MaxHeight(k1->left,k2->right)+1;

    k2->height=MaxHeight(k1,k2->right)+1;

    return k2;

}

static Position DRL(Position k3)

{

    k3->left=SRR(k3->left);

    return SRL(k3);

}

static Position DRR(Position k3)

{

    k3->right=SRL(k3->right);

    return SRR(k3);

}

//header

#ifndef AVLTREE_H_INCLUDED

#define AVLTREE_H_INCLUDED

#include <iostream>

#include <cstring>

using namespace std;

struct information

{

    string name;

    string phonenumber;

    string address;



};

typedef struct AVLNode

{

    information *data;

    AVLNode *left;

    AVLNode *right;

    int height;

}*Node,*Position,*AVLTree;

AVLTree MakeEmpty(AVLTree T);

void display(Position p);

int Max(Position p1,Position p2);

int MaxHeight(Position p1,Position p2);

AVLTree Insert(string name,string number,string address,AVLTree T);

static Position SRL(Position k2);//Single rotate with left

static Position SRR(Position k1);//Single rotate with right

static Position DRL(Position k3);

static Position DRR(Position k3);



#endif // AVLTREE_H_INCLUDED
