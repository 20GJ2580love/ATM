#include"stdio.h"
#include"stdlib.h"
#include"string.h"
#include "time.h"

void start();
//中文
void C_Sign_Up();//中文注册
void C_Sign_In();//中文登录 
void C_Main_Menu();//中文菜单页

void C_DepositMoney();//存款业务
void C_Withdraw_Money();//取款业务
void C_Transfer_Money();//转款业务

void C_Enquiry();//查询业务
void C_Enquiry_balance();//查询余额 
void C_Enquiry_Information();//查询个人信息
void C_Enquiry_Record();//查询流水记录

void C_Change_Information();//修改信息选项页
void C_Change_User();//修改用户名称
void C_Change_AccountNum();//修改账户
void C_Change_phone();//修改联系电话
void C_Change_Password();//修改密码

//English
void E_Sign_Up();//英文注册
void E_Sign_In();//英文登录 
void E_Main_Menu();//中文菜单页

void E_DepositMoney();//存款业务
void E_Withdraw_Money();//取款业务
void E_Transfer_Money();//转账业务

void E_Enquiry();//查询业务
void E_Enquiry_balance();//查询余额 
void E_Enquiry_Information();//查询个人信息
void E_Enquiry_Record();//查询流水记录

void E_Change_Information();//修改信息选项页
void E_Change_User();//修改用户名称
void E_Change_AccountNum();//修改账户
void E_Change_phone();//修改联系电话
void E_Change_Password();//修改密码

//CE
void CE_Time();//系统时间
void CE_Close_Account();//注销账户
void CE_Close_Account1();
void CE_Save_User_Data();       //保存用户数据
void CE_Save_Transaction_Data();//保存交易数据
void CE_Initial_Password();           //初始化密码函数
void CE_Initialize_User_Data();       //初始化用户数据
void CE_Initialize_Transaction_Data();//初始化交易数据
void CE_Set_Name();          //中英文设置姓名
void CE_Set_Password();      //中英文设置密码
void CE_Set_PhoneNumber();   //中英文设置电话号码
void CE_Add_Account_Number();//中英文设置账户号码函数
void CE_Deposit_Transaction(); //存款交易
void CE_Withdraw_Transaction();//取款交易
void CE_Transfer_Transaction();//转款交易
void CE_Print_Transaction();//打印交易记录

int Language;
int Bank;
int Random_Number;//随机数
char str[50];
int AccountNum1 = 5001000;
int AccountNum2 = 5002000;
int AccountNum3 = 5003000;

//用户信息
struct Account
{
	//char bank[100];//银行
	char name[100];//用户名称
	char account_number[50];//账户账号
	char phone_number[12];//电话号码
	char password[10];//密码
	float balance;//余额 

	struct Account* next;
};

//交易信息
struct Trade
{
	char Taccount_number[50];//账户账号
	char Time[100];//时间
	char Operation[100];//操作类型
	char TarUse[50];//目标账户账号
	float Money;//操作数量

	struct Trade* Tnext;
};

typedef struct Account Account;
typedef struct Trade Trade;

Account* Pset = (Account*)malloc(sizeof(Account));
Account* head = NULL;
Account* tail = NULL;
Account* curAccount;
Account* curAccountO;
Account* curAccount1;
Account* curAccount2;
Account* curAccount3;
Account IV;

Trade* Thead = NULL;
Trade* Ttail = NULL;
Trade* TcurAccount;
Trade TA;
char tmp1[100];

//查找当前账户
int findAccount(Account IV1)//登录时查找当前账户
{
	Account* curP = head;
	Trade* TcurP = Thead;
	while (curP != NULL)
	{
		if (strcmp(curP->account_number, IV1.account_number) == 0)
		{
			return 1;
		}
		curP = curP->next;
	}
	return 0;
}

//验证当前账户密码
int findPassword(Account IV1)//登录时查找当前账户密码
{
	Account* curP = head;
	while (curP != NULL)
	{
		if (strcmp(curP->account_number, IV1.account_number) == 0 && strcmp(curP->password, IV1.password) == 0)
		{
			curAccount = curP;
			return 1;
		}
		curP = curP->next;
	}
	return 0;
}

//查找是否被已有账户
int findAccount2(Account IV)
{
	Account* curP = head;
	while (curP != NULL)
	{
		if (strcmp(curP->account_number, IV.account_number) == 0)
		{
			return 1;
		}
		curP = curP->next;
	}
	return 0;
}

//查找转账对象
int findAccount3(Account TT)
{
	Account* curP = head;
	while (curP != NULL)
	{
		if (strcmp(curAccount->account_number, TT.account_number) == 0)
		{
			int b, k = 0;
			if (Language == 1)
				printf("\n不能给自己转账！！！\n");
			else
				printf("\nYou can't transfer money to yourself! ! !\n");
			while (k < 3)
			{
				if (Language == 1)
				{
					printf("\n\n\n\t\t【1】重新输入对方账户号码");
					printf("\n\n\n\t\t【2】返回主页");
					printf("\n\n\n\t\t【0】退出系统\n\n");
				}
				else
				{
					printf("\n\n\n\t\t【1】Re-enter the account number of the other party");
					printf("\n\n\n\t\t【2】Return to the homepage");
					printf("\n\n\n\t\t【0】Exit the system\n\n");
				}
				scanf("%d", &b);
				switch (b)
				{
				case 1:
					system("cls");
					C_Transfer_Money();
					k = 3;
					break;
				case 2:
					system("cls");
					C_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
		else if (strcmp(curP->account_number, TT.account_number) == 0)
		{
			curAccountO = curP;//当前转账对象账户
			return 1;
		}
		curP = curP->next;
	}
	return 0;
}

void CE_Time()
{
	time_t t = time(0);
	Random_Number = time(0) % 4 + 1;
	char tmp[20];
	strftime(tmp, sizeof(tmp), "%Y/%m/%d/%X", localtime(&t));
	strftime(tmp1, sizeof(tmp), "%Y-%m-%d", localtime(&t));
	strcpy(TA.Time, tmp);
}

void CE_Close_Account()
{
	printf("\n\n\t此界面为注销账户界面，是否继续注销该账户？\n\n\t【温馨提示】1.注销前请确认余额是否清0，如果账户余额未清0，账号将无法注销成功。\n\n\t\t2.如果继续注销账号将会被回收，届时您将无法再次登录该账号。");
	int aaa, k1 = 1;
	while (k1)
	{
		k1 = 0;
		printf("\n\n\t\t【1】继续注销\n");
		printf("\n\n\t\t【2】不了，返回主页\n");
		printf("\n\n\t\t【0】退出系统\n");
		scanf("%d", &aaa);
		switch (aaa)
		{
		case 1:
			system("cls");
			CE_Close_Account1();
			break;
		case 2:
			system("cls");
			C_Main_Menu();
			break;
		case 0:
			system("cls");
			CE_Save_User_Data();
			exit(0);
			break;
		default:
			system("cls");
			printf("\n\n\t\t输入错误！\n");
			k1 = 1;
			break;
		}
	}
}

void CE_Close_Account1()
{
	if (curAccount->balance != 0)
	{
		printf("\n\n\t\t注销失败：余额未清零。\n");
		int a, k1 = 1;
		while (k1)
		{
			k1 = 0;
			printf("\n\n\t\t【1】去转账\n");
			printf("\n\n\t\t【2】返回主页\n");
			printf("\n\n\t\t【0】退出系统\n");
			scanf("%d", &a);
			switch (a)
			{
			case 1:
				system("cls");
				C_Transfer_Money();
				break;
			case 2:
				system("cls");
				C_Main_Menu();
				break;
			case 0:
				system("cls");
				CE_Save_User_Data();
				exit(0);
				break;
			default:
				system("cls");
				printf("\n\n\t\t输入错误！\n");
				k1 = 1;
				break;
			}
		}
	}
	else
	{
		printf("\n\n\t\t注销成功，欢迎您下次光临。\n");
		strcpy(curAccount->name, "已被注销");
		strcpy(curAccount->account_number, "已被注销");
		strcpy(curAccount->phone_number, "已被注销");
		strcpy(curAccount->password, "已被注销");
		CE_Save_User_Data();
		//free(curAccount);
		exit(0);
	}

}

//保存用户数据
void CE_Save_User_Data()
{
	FILE* fp = fopen("D:/atm.txt", "w");//保存用户数据
	if (fp != NULL)
	{
		Account* curP = head;
		while (curP != NULL)
		{
			fprintf(fp, "%s\t%s\t%s\t%s\t%f\n", curP->account_number, curP->name, curP->phone_number, curP->password, curP->balance);
			curP = curP->next;
		}
		fclose(fp);
	}
}

//保存交易数据
void CE_Save_Transaction_Data()
{
	FILE* Tfp = fopen("D:/atmtrade.txt", "at+");//保存交易数据
	if (Tfp != NULL)
	{
		Trade* TcurP = Thead;
		while (TcurP != NULL)
		{
			if (TcurP->Tnext == NULL)
			{
				fprintf(Tfp, "%s\t%s\t%s\t%s\t%f\n", TcurP->Taccount_number, TcurP->Time, TcurP->Operation, TcurP->TarUse, TcurP->Money);
			}
			TcurP = TcurP->Tnext;
		}
		fclose(Tfp);
	}
}

//初始化用户信息
void CE_Initialize_User_Data()
{
	FILE* fp = fopen("D:/atm.txt", "r");
	if (fp != NULL)
	{
		while (!feof(fp))
		{
			Account* newNodeP = (Account*)malloc(sizeof(Account));
			fscanf(fp, "%s\t%s\t%s\t%s\t%f\n", newNodeP->account_number, newNodeP->name, newNodeP->phone_number, newNodeP->password, &newNodeP->balance);
			newNodeP->next = NULL;

			if (head == NULL)
			{
				head = newNodeP;
				tail = newNodeP;
			}
			else
			{
				tail->next = newNodeP;
				tail = newNodeP;
			}
		}
		fclose(fp);
	}
}

//初始化交易信息
void CE_Initialize_Transaction_Data()
{
	FILE* Tfp = fopen("D:/atmtrade.txt", "r");
	if (Tfp != NULL)
	{
		while (!feof(Tfp))
		{
			Trade* TnewNodeP = (Trade*)malloc(sizeof(Trade));
			fscanf(Tfp, "%s\t%s\t%s\t%s\t%f\n", TnewNodeP->Taccount_number, TnewNodeP->Time, TnewNodeP->Operation, TnewNodeP->TarUse, &TnewNodeP->Money);
			TnewNodeP->Tnext = NULL;

			if (Thead == NULL)
			{
				Thead = TnewNodeP;
				Ttail = TnewNodeP;
			}
			else
			{
				Ttail->Tnext = TnewNodeP;
				Ttail = TnewNodeP;
			}
		}
		fclose(Tfp);
	}
}

//初始化密码
void CE_Initial_Password()
{
	strcpy(curAccount->password, "000000");
}

//中英文设置姓名
void CE_Set_Name()
{
	int a = 0;
	while (a < 3)
	{
		a++;
		if (Language == 1)
			printf("\n\t>>请输入你的姓名：\n");
		else
			printf("\n\t>>Please enter your name:\n");
		scanf("%s", IV.name);
		if (strlen(IV.name) < 100)
		{
			strcpy(Pset->name, IV.name);
			break;
		}
		else
		{
			system("cls");
			if (Language == 1)
				printf("\n\t\t你的姓名设置过长。\n\n\t请重新设置：\n");
			else
				printf("\n\t\tYour name is set too long.\n\n\tPlease reset it:\n");
		}
	}
}

//中英文设置密码
void CE_Set_Password()
{
	int a = 0;
	while (a < 3)
	{
		a++;
		if (Language == 1)
			printf("\n\t>>请设置你的账户密码【6位】：\n");
		else
			printf("\n\t>>Please set your account password [6 digits]：\n");
		scanf("%s", IV.password);
		if (strlen(IV.password) == 6)
		{
			strcpy(Pset->password, IV.password);
			break;
		}
		else
		{
			system("cls");
			if (Language == 1)
				printf("\n\t\t密码为六位，你的密码设置有误！\n\n\t请重新设置：\n");
			else
				printf("\n\t\tThe password is six digits, your password is set incorrectly！\n\n\tPlease reset it:\n");
		}
	}
}

//中英文设置电话号
void CE_Set_PhoneNumber()
{
	int a = 0;
	while (a < 3)
	{
		a++;
		if (Language == 1)
			printf("\n\t>>请输入你的电话号码【11位】：\n");
		else
			printf("\n\t>>Please enter your phone number [11 digits]:\n");
		scanf("%s", IV.phone_number);
		if (strlen(IV.phone_number) == 11)
		{
			strcpy(Pset->phone_number, IV.phone_number);
			break;
		}
		else
		{
			system("cls");
			if (Language == 1)
				printf("\n\t\t预留号码应为十一位数，你的预留号码设置有误！\n\n\t请重新输入：\n");
			else
				printf("\n\t\tThe reserved number should be eleven digits, your reserved number is set incorrectly!\n\n\tPlease reset it:\n");
		}
	}
}

//中英文增加账户账号
void CE_Add_Account_Number()
{
	switch (Bank)
	{
	case 1:
		AccountNum1++;
		itoa(AccountNum1, IV.account_number, 10);
		break;
	case 2:
		AccountNum2++;
		itoa(AccountNum2, IV.account_number, 10);
		break;
	case 3:
		AccountNum3++;
		itoa(AccountNum3, IV.account_number, 10);
		break;
	}
	while (findAccount2(IV))
	{
		switch (Bank)
		{
		case 1:
			AccountNum1++;
			itoa(AccountNum1, IV.account_number, 10);
			break;
		case 2:
			AccountNum2++;
			itoa(AccountNum2, IV.account_number, 10);
			break;
		case 3:
			AccountNum3++;
			itoa(AccountNum3, IV.account_number, 10);
			break;
		}

	}
}

//中英文存豆流水记录
void CE_Deposit_Transaction()
{
	Trade* TP = (Trade*)malloc(sizeof(Trade));
	TP->Tnext = NULL;
	if (Thead == NULL)
	{
		Thead = TP;
		Ttail = TP;
	}
	else
	{
		Ttail->Tnext = TP;
		Ttail = TP;//我滴天哪
		strcpy(TP->Taccount_number, curAccount->account_number);
		strcpy(TP->Time, TA.Time);
		strcpy(TP->Operation, "B存豆/收入");
		TP->Money = TA.Money;
		strcpy(TP->TarUse, "本账户");
		CE_Save_Transaction_Data();
	}
}

//中英文取豆流水记录
void  CE_Withdraw_Transaction()
{
	Trade* TP = (Trade*)malloc(sizeof(Trade));
	TP->Tnext = NULL;
	if (Thead == NULL)
	{
		Thead = TP;
		Ttail = TP;
	}
	else
	{
		Ttail->Tnext = TP;
		Ttail = TP;//我滴天哪
		strcpy(TP->Taccount_number, curAccount->account_number);
		strcpy(TP->Time, TA.Time);
		strcpy(TP->Operation, "A取豆/支出");
		TP->Money = TA.Money;
		strcpy(TP->TarUse, "本账户");
		CE_Save_Transaction_Data();
	}
}

//中英文转账转出流水记录
void  CE_Transfer_Transaction()
{
	Trade* TP = (Trade*)malloc(sizeof(Trade));
	TP->Tnext = NULL;
	if (Thead == NULL)
	{
		Thead = TP;
		Ttail = TP;
	}
	else
	{
		Ttail->Tnext = TP;
		Ttail = TP;//我滴天哪
		strcpy(TP->Taccount_number, curAccount->account_number);
		strcpy(TP->Time, TA.Time);
		strcpy(TP->Operation, "A转豆/支出");
		strcpy(TP->TarUse, curAccountO->account_number);
		TP->Money = TA.Money;
		CE_Save_Transaction_Data();
	}

	//转账转入流水记录

	Trade* TPi = (Trade*)malloc(sizeof(Trade));//定义交易结构体 变量 转入TPi
	TPi->Tnext = NULL;
	if (Thead == NULL)
	{
		Thead = TPi;
		Ttail = TPi;
	}
	else
	{
		Ttail->Tnext = TPi;
		Ttail = TPi;//我滴天哪
		strcpy(TPi->Taccount_number, curAccountO->account_number);//当前转入账号赋值给TPi
		strcpy(TPi->Time, TA.Time);//当前时间赋值给TPi
		strcpy(TPi->Operation, "B转豆/收入");//当前操作类型赋值给TPi
		strcpy(TPi->TarUse, curAccount->account_number);//当前转出账号赋值给TPi
		TPi->Money = -TA.Money;//当前交易金额赋值给TPi
		CE_Save_Transaction_Data();
	}
}

//中英文打印流水
void CE_Print_Transaction()
{
	int k = 0, w = 0;
	Trade* TcurP = Thead;
	if (Language == 1)
		printf("\n序号\t时间：\t\t\t\t交易类型：\t\t交易欢乐豆数量：\t交易账户：\n\n");
	else
		printf("\nNum:\tTime:\t\t\t\tTrading Type:\t\tTrading Bean quantity:\tAccount:\n\n");
	while (TcurP != NULL)//交易记录
	{
		if (strcmp(curAccount->account_number, TcurP->Taccount_number) == 0)
		{
			w++;
			TcurAccount = TcurP;
			if (TcurAccount->Money < 0)
			{
				printf("\n<%d>\t%s\t\t%s\t\t%.2fo\t\t%s\t||\n", w, TcurAccount->Time, TcurAccount->Operation, TcurAccount->Money, TcurAccount->TarUse);
			}
			else
			{
				printf("\n<%d>\t%s\t\t%s\t\t+%.2fo\t\t%s\t||\n", w, TcurAccount->Time, TcurAccount->Operation, TcurAccount->Money, TcurAccount->TarUse);
			}
			k++;
		}
		TcurP = TcurP->Tnext;
	}
	if (k == 0)
	{
		if (Language == 1)
			printf("\n\t暂=无=交=易=记=录\n\t\t\n\n");
		else
			printf("\n\tNo transaction record now.\n\t\t\n\n");
	}
}

//
void CE_ChooseBank()
{
	int a, k = 1;
	while (k)
	{
		k = 0;
		printf("\n请选择你要注册账号的所属很行：\n\n");
		printf("\n\n\t\t【1】人民银行\n");
		printf("\n\n\t\t【2】工商银行\n");
		printf("\n\n\t\t【3】邮政银行\n");
		printf("\n\n\t\t【4】农业银行\n");
		printf("\n\n\t\t【0】退出系统\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			Bank = 1;
			break;
		case 2:
			Bank = 2;
			break;
		case 3:
			Bank = 3;
			break;
		case 4:
			Bank = Random_Number;
			break;
		case 0:
			system("cls");
			CE_Save_User_Data();
			exit(0);
			break;
		default:
			system("cls");
			printf("\n\n\t\t输入错误！\n");
			k = 1;
			break;
		}
	}
}

//中文注册
void C_Sign_Up()//中文注册函数
{
	int a, k = 0;
	printf("\n===============欢迎注册===============\n\n");

	CE_ChooseBank();
	Pset->next = NULL;//Account* Pset = (Account*)malloc(sizeof(Account));//已定义
	if (head == NULL)
	{
		head = Pset;
		tail = Pset;//如果这是第一个创建的结点，则将头 尾指针指向这个结点
	}
	else
	{
		tail->next = Pset; //如果不是第一个创建的结点，则将上一个结点的后继指针指向当前结点
		CE_Set_Name();//跳转中文设置姓名函数
		CE_Add_Account_Number();//跳转到中文设置账户号码函数
		strcpy(Pset->account_number, IV.account_number);
		CE_Set_PhoneNumber();//跳转到中文设置预留电话函数
		CE_Set_Password();//跳转到中文设置密码函数
		Pset->balance = 0;
		CE_Save_User_Data();
		while (k < 3)
		{
			system("cls");
			printf("\n\t\t注=册=成=功\n");
			printf("\n\t>>你的账户号码为1000+%s\n", Pset->account_number);
			printf("\n\n\t\t【1】返回登录\n");
			printf("\n\n\t\t【0】退出系统\n");
			scanf("%d", &a);
			if (a == 1)
			{
				system("cls");
				C_Sign_In();
				break;
			}
			else if (a == 0)
			{
				exit(0);
			}
			else
			{
				k++;
				printf("\n密码输入错误! \t%d\t请按照提示重新输入：\n", 3 - k);
			}
		}
	}
}

//英语注册
void E_Sign_Up()//英语注册函数
{
	int a, k = 0;
	printf("\n==============Welcome to register================\n\n");

	CE_ChooseBank();
	Pset->next = NULL;
	if (head == NULL)
	{
		head = Pset;
		tail = Pset;
	}
	else
	{
		tail->next = Pset;
		CE_Set_Name();
		CE_Add_Account_Number();
		strcpy(Pset->account_number, IV.account_number);
		CE_Set_PhoneNumber();
		CE_Set_Password();
		Pset->balance = 0;
		CE_Save_User_Data();
		while (k < 3)
		{
			system("cls");
			printf("\n\t\tRegistration Success!\n");
			printf("\n\t>>Your account number is %s\n", Pset->account_number);
			printf("\n\n\t\t【1】 Return to login\n");
			printf("\n\n\t\t【0】Exit the system\n");
			scanf("%d", &a);
			if (a == 1)
			{
				system("cls");
				E_Sign_In();
				break;
			}
			else if (a == 0)
			{
				exit(0);
			}
			else
			{
				k++;
				printf("\nPassword entered incorrectly! \t%d\tPlease re-enter as prompted:\n", 3 - k);
			}
		}
	}
}

//中文登录
void C_Sign_In()
{
	system("cls");
	int m = 0, n = 0;
	while (m < 3)//3次输入错账号将退出系统
	{
		Account IV1;//账户中间变量1
		printf("\n登录\n========================================\n");
		printf("\n\n\n\t>>请输入账号：\n");
		scanf("%s", IV1.account_number);
		system("cls");
		if (findAccount(IV1))
		{
			while (n < 3)//3次输入错密码将退出系统
			{
				printf("\n登录\n========================================\n");
				printf("\n\n\n\t>>请输入密码：\n");
				
				scanf("%s", IV1.password);
				system("cls");

				if (findPassword(IV1))
				{
					m = 3;
					n = 3;
					printf("\n菜单\n========================================\n");
					C_Main_Menu();
					break;
				}
				else
				{
					m++;
					n++;
					if (n == 3)
					{
						printf("\n\n\t该账号已经被冻结，请联系后台找回。\n");
						CE_Initial_Password();
						break;
					}
					else
					{
						printf("\n\n\t密码错误  %d次机会后账号将会被冻结\n", 3 - n);
					}
				}
			}
		}
		else
		{
			m++;
			printf("\n\n\t没有查找到该用户!  %d  请重新输入：\n", 3 - m);
			if (m == 3)
			{
				printf("\n\n\t错误，已退出系统\n");
				CE_Save_User_Data();
				exit(0);
			}
		}
	}
}

//英语登录
void E_Sign_In()
{
	system("cls");
	int m = 0, n = 0;
	while (m < 3)//3次输入错账号将退出系统
	{
		Account IV1;//账户中间变量1
		printf("\nSign in\n==========================================\n");
		printf("\n\n\t>>Please enter account number:\n");
		scanf("%s", IV1.account_number);
		system("cls");
		if (findAccount(IV1))
		{
			while (n < 3)//3次输入错密码将退出系统
			{
				printf("\nSign in\n========================================\n\n\n\t>>>>Please enter password:\n");
				scanf("%s", IV1.password);
				system("cls");

				if (findPassword(IV1))
				{
					m = 3;
					n = 3;
					printf("\nMenu\n========================================\n");
					E_Main_Menu();
					break;
				}
				else
				{
					m++;
					n++;
					if (n == 3)
					{
						printf("\n\n\tThe account has been frozen, please contact the background to retrieve it.\n");
						CE_Initial_Password();
						break;
					}
					else
					{
						printf("\n\n\tThe password is wrong, the account will be frozen after %d chance\n", 3 - n);
					}
				}
			}
		}
		else
		{
			m++;
				printf("\n\n\tThe user was not found! %d Please re-enter:\n", 3 - m);
			if (m == 3)
			{
				printf("\n\n\tError, logged out\n");
				CE_Save_User_Data();
				exit(0);
			}
		}
	}
}

//中文菜单页
void C_Main_Menu()//
{
	int a, k1 = 1;
	while (k1)
	{
		k1 = 0;
		printf("\n\n\t\t【1】存款\n");
		printf("\n\n\t\t【2】取款\n");
		printf("\n\n\t\t【3】转帐\n");
		printf("\n\n\t\t【4】查询\n");
		printf("\n\n\t\t【5】修改个人资料\n");
		printf("\n\n\t\t【6】注销账号\n");
		printf("\n\n\t\t【0】退出系统\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			C_DepositMoney();
			break;
		case 2:
			system("cls");
			C_Withdraw_Money();
			break;
		case 3:
			system("cls");
			C_Transfer_Money();
			break;
		case 4:
			system("cls");
			C_Enquiry();
			break;
		case 5:
			system("cls");
			C_Change_Information();
			break;
		case 6:
			system("cls");
			CE_Close_Account();
			break;
		case 0:
			system("cls");
			CE_Save_User_Data();
			exit(0);
			break;
		default:
			system("cls");
			printf("\n\n\t\t输入错误！\n");
			k1 = 1;
			break;
		}
	}
}

//英语菜单页
void E_Main_Menu()//
{
	int a, k1 = 1;
	while (k1)
	{
		k1 = 0;
		printf("\n\n\t\t【1】Deposit money\n");
		printf("\n\n\t\t【2】Withdraw money\n");
		printf("\n\n\t\t【3】Transfer money\n");
		printf("\n\n\t\t【4】Inquire\n");
		printf("\n\n\t\t【5】Modify personal information\n");
		printf("\n\n\t\t【6】Logout\n");
		printf("\n\n\t\t【0】Exit system\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			E_DepositMoney();
			break;
		case 2:
			system("cls");
			E_Withdraw_Money();
			break;
		case 3:
			system("cls");
			E_Transfer_Money();
			break;
		case 4:
			system("cls");
			E_Enquiry();
			break;
		case 5:
			system("cls");
			E_Change_Information();
			break;
		case 6:
			system("cls");
			CE_Close_Account();
			break;
		case 0:
			system("cls");
			CE_Save_User_Data();
			exit(0);
			break;
		default:
			system("cls");
			printf("\n\n\t\tIncorrect input!\n");
			k1 = 1;
			break;
		}
	}
}

//中文存豆业务
void C_DepositMoney()
{
	int a;
	printf("\n【存豆】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	printf("\n\n\n\t【温馨提示】1.金额数量须为100的整数。\n\n\t\t2.单笔存款不得超过10000。");
	printf("\n\n\n\t>>请输入你要存入的金额：\n");
	scanf("%d", &a);
	if (a % 100 || a > 10000)
	{
		int k = 0, b;
		if (a > 10000)
		{
			printf("单笔存款不得超过10000。\n\n");
		}
		else
			printf("金额数额须为100的整数。\n\n");
		while (k < 3)
		{
			printf("\n\n\t\t【1】重新输入数额\n");
			printf("\n\n\t\t【2】返回主页\n");
			printf("\n\n\t\t【0】退出系统\n");
			scanf("%d", &b);
			switch (b)
			{
			case 1:
				system("cls");
				C_DepositMoney();
				k = 3;
				break;
			case 2:
				system("cls");
				C_Main_Menu();
				k = 3;
				break;
			case 0:
				k = 3;
				CE_Save_User_Data();
				exit(0);
			default:
				k++;
				break;
			}
		}
	}
	else
	{
		CE_Time();
		TA.Money = a * 1.00;
		curAccount->balance = curAccount->balance + TA.Money;
		printf("\n您本次已成功存入%.2f!!!!\n", TA.Money);
		CE_Deposit_Transaction();
		int k = 0, b;
		while (k < 3)
		{
			printf("\n\n\t\t【1】再存一笔\n");
			printf("\n\n\t\t【2】返回主页\n");
			printf("\n\n\t\t【0】退出系统\n");
			scanf("%d", &b);
			switch (b)
			{
			case 1:
				system("cls");
				C_DepositMoney();
				k = 3;
				break;
			case 2:
				system("cls");
				C_Main_Menu();
				k = 3;
				break;
			case 0:
				k = 3;
				CE_Save_User_Data();
				exit(0);
			default:
				k++;
				break;
			}
		}
	}
}

//英语存豆业务
void E_DepositMoney()
{
	int a;
	printf("\n[Save Beans]\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	printf("\n\n\n\t[Reminder] 1. The number of happy beans must be an integer of 100.\n\n\t\t2. A single deposit of beans cannot exceed 10,000.");
	printf("\n\n\n\t>>Please enter the happy bean you want to deposit:\n");
	scanf("%d", &a);
	if (a % 100 || a > 10000)
	{
		int k = 0, b;
		if (a > 10000)
		{
			printf("Single deposit must not exceed 10000.\n\n");
		}
		else
			printf("The amount of happy beans must be an integer of 100.\n\n");
		while (k < 3)
		{
			printf("\n\n\t\t[1] Re-enter the amount of happy beans\n");
			printf("\n\n\t\t【2】Return to homepage\n");
			printf("\n\n\t\t【0】Exit the system\n");
			scanf("%d", &b);
			switch (b)
			{
			case 1:
				system("cls");
				E_DepositMoney();
				k = 3;
				break;
			case 2:
				system("cls");
				E_Main_Menu();
				k = 3;
				break;
			case 0:
				k = 3;
				CE_Save_User_Data();
				exit(0);
			default:
				k++;
				break;
			}
		}
	}
	else
	{
		CE_Time();
		TA.Money = a * 1.00;
		curAccount->balance = curAccount->balance + TA.Money;
		printf("\nYou have successfully deposited %.2f Happy Beans!!!!\n", TA.Money);
		CE_Deposit_Transaction();
		int k = 0, b;
		while (k < 3)
		{
			printf("\n\n\t\t【1】Save another one\n");
			printf("\n\n\t\t【2】Return to homepage\n");
			printf("\n\n\t\t【0】Exit the system\n");
			scanf("%d", &b);
			switch (b)
			{
			case 1:
				system("cls");
				E_DepositMoney();
				k = 3;
				break;
			case 2:
				system("cls");
				E_Main_Menu();
				k = 3;
				break;
			case 0:
				k = 3;
				CE_Save_User_Data();
				exit(0);
			default:
				k++;
				break;
			}
		}
	}
}

//中文取豆业务
void C_Withdraw_Money()
{
	int a;
	printf("\n【取豆】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	printf("\n\n\n\t\t【温馨提示】1.金额数量须为100的整数。");
	printf("\n\n\n\t\t\t2.单笔取款不得超过10000。");
	printf("\n\n\n\t\t>>请输入你要取出的欢乐款：\n\n");
	scanf("%d", &a);
	if (a % 100 || a > 10000)
	{
		int k = 0, b;
		if (a > 10000)
		{
			printf("单笔存款不得超过10000。\n\n");
		}
		else
			printf("数额须为100的整数。\n\n");
		while (k < 3)
		{
			printf("\n\n\n\t\t【1】重新输入数额");
			printf("\n\n\n\t\t【2】返回主页");
			printf("\n\n\n\t\t【0】退出系统");
			scanf("%d", &b);
			switch (b)
			{
			case 1:
				system("cls");
				C_Withdraw_Money();
				k = 3;
				break;
			case 2:
				system("cls");
				C_Main_Menu();
				k = 3;
				break;
			case 0:
				k = 3;
				CE_Save_User_Data();
				exit(0);
			default:
				k++;
				break;
			}
		}
	}
	else
	{
		CE_Time();
		TA.Money = a * -1.00;
		if (curAccount->balance + TA.Money < 0)
		{
			int k = 0, b;
			printf("\n余额不足!!!!\n");
			while (k < 3)
			{
				printf("\n\n\n\t\t【1】重新输入数额");
				printf("\n\n\n\t\t【2】返回主页");
				printf("\n\n\n\t\t【0】退出系统\n");
				scanf("%d", &b);
				switch (b)
				{
				case 1:
					system("cls");
					C_Withdraw_Money();
					k = 3;
					break;
				case 2:
					system("cls");
					C_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
		else
		{
			curAccount->balance = curAccount->balance + TA.Money;
			printf("\n取款成功!!!!\n");
			CE_Withdraw_Transaction();
			int k = 0, b;
			while (k < 3)
			{
				printf("\n\n\n\t\t【1】再次取款");
				printf("\n\n\n\t\t【2】返回主页");
				printf("\n\n\n\t\t【0】退出系统\n\n");
				scanf("%d", &b);
				switch (b)
				{
				case 1:
					system("cls");
					C_Withdraw_Money();
					k = 3;
					break;
				case 2:
					system("cls");
					C_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
	}
}

//英语取豆业务
void E_Withdraw_Money()
{
	int a;
	printf("\n【Take Beans】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	printf("\n\n\n\t\t[Reminder] 1. The number of happy beans must be an integer of 100.");
	printf("\n\n\n\t\t\t2. A single fetch must not exceed 10000.");
	printf("\n\n\n\t\t>>Please enter the happy bean you want to take out:\n\n");
	scanf("%d", &a);
	if (a % 100 || a > 10000)
	{
		int k = 0, b;
		if (a > 10000)
		{
			printf("Single deposit must not exceed 10000.\n\n");
		}
		else
			printf("The amount of happy beans must be an integer of 100.\n\n");
		while (k < 3)
		{
			printf("\n\n\n\t\t【1】 Re-enter the amount of happy beans");
			printf("\n\n\n\t\t【2】 Return to home page");
			printf("\n\n\n\t\t【0】 Exit the system");
			scanf("%d", &b);
			switch (b)
			{
			case 1:
				system("cls");
				E_Withdraw_Money();
				k = 3;
				break;
			case 2:
				system("cls");
				E_Main_Menu();
				k = 3;
				break;
			case 0:
				k = 3;
				CE_Save_User_Data();
				exit(0);
			default:
				k++;
				break;
			}
		}
	}
	else
	{
		CE_Time();
		TA.Money = a * -1.00;
		if (curAccount->balance + TA.Money < 0)
		{
			int k = 0, b;
			printf("\nNot enough happy beans!!!!\n");
			while (k < 3)
			{
				printf("\n\n\n\t\t【1】 Re-enter the amount of happy beans");
				printf("\n\n\n\t\t【2】 Return to home page");
				printf("\n\n\n\t\t【0】 Exit the system\n");
				scanf("%d", &b);
				switch (b)
				{
				case 1:
					system("cls");
					E_Withdraw_Money();
					k = 3;
					break;
				case 2:
					system("cls");
					E_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
		else
		{
			curAccount->balance = curAccount->balance + TA.Money;
			printf("\nGet the beans successfully!!!!\n");
			CE_Withdraw_Transaction();
			int k = 0, b;
			while (k < 3)
			{
				printf("\n\n\n\t\t【1】 Take beans again");
				printf("\n\n\n\t\t【2】 Return to home page");
				printf("\n\n\n\t\t【0】 Exit the system\n\n");
				scanf("%d", &b);
				switch (b)
				{
				case 1:
					system("cls");
					E_Withdraw_Money();
					k = 3;
					break;
				case 2:
					system("cls");
					E_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
	}
}

//转豆业务
void C_Transfer_Money()
{
	int a = 100;
	printf("\n【转帐】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	printf("\n\n\n\t【温馨提示】1.单笔转账不得超过50000金额。\n");
	printf("\n\n\t\t2.汇款短信莫当真，陌生电话要谨慎。");
	printf("\n\n\t\t  遇到陷阱知深浅，正心律己要实诚。");
	printf("\n\n\t\t>>请输入你要转出到的账户：\n\n");
	Account TT;//目标账户
	scanf("%s", &TT.account_number);
	if (findAccount3(TT))
	{
		printf(">>请输入你要转出的金额：\n");
		scanf("%d", &a);
		if (a > 10000)
		{
			printf("\n\t单笔转帐不得超过50000\n\t\t\n\n");
			C_Transfer_Money();
		}
		CE_Time();
		//strcpy(TA.Operation, "B转账");
		TA.Money = a * -1.00;
		if (curAccount->balance + TA.Money < 0)
		{
			int k = 0, b;
			printf("\n欢乐豆不足!!!!\n");
			while (k < 3)
			{
				printf("\n\n\n\t\t【1】重新输入数额");
				printf("\n\n\n\t\t【2】返回主页");
				printf("\n\n\n\t\t【0】退出系统\n");
				scanf("%d", &b);
				switch (b)
				{
				case 1:
					system("cls");
					C_Transfer_Money();
					k = 3;
					break;
				case 2:
					system("cls");
					C_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
		else
		{
			printf("\n转帐成功!!!!\n");
			curAccount->balance = curAccount->balance + TA.Money;//当前账户减去转账金额
			curAccountO->balance = curAccountO->balance - TA.Money;//对方账户加上转账金额
			CE_Transfer_Transaction();
			int k = 0, b;
			while (k < 3)
			{
				printf("\n\n\n\t\t【1】再次转帐");
				printf("\n\n\n\t\t【2】返回主页");
				printf("\n\n\n\t\t【0】退出系统\n\n");
				scanf("%d", &b);
				switch (b)
				{
				case 1:
					system("cls");
					C_Transfer_Money();
					k = 3;
					break;
				case 2:
					system("cls");
					C_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
	}
	else
	{
		int b, k = 0;
		printf("\n对方账户不存在！！！！");
		while (k < 3)
		{
			printf("\n\n\n\t\t【1】重新输入对方账号");
			printf("\n\n\n\t\t【2】返回主页");
			printf("\n\n\n\t\t【0】退出系统\n\n");
			scanf("%d", &b);
			switch (b)
			{
			case 1:
				system("cls");
				C_Transfer_Money();
				k = 3;
				break;
			case 2:
				system("cls");
				C_Main_Menu();
				k = 3;
				break;
			case 0:
				k = 3;
				CE_Save_User_Data();
				exit(0);
			default:
				k++;
				break;
			}
		}
	}
}

//转豆业务
void E_Transfer_Money()
{
	int a = 100;
	printf("\n【Send Beans】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	printf("\n\n\n\t【Reminder】1. No more than 10000 happy beans can be sent at a time.\n");
	printf("\n\n\t\t2.Beware of scams.");
	printf("\n\n\t\t>>Please enter the account you want to send to:\n\n");
	Account TT;
	scanf("%s", &TT.account_number);
	if (findAccount3(TT))
	{
		printf(">>Please enter the amount of Happy Beans you want to send:\n");
		scanf("%d", &a);
		if (a > 10000)
		{
			printf("\n\tSingle delivery of beans must not exceed 10000\n\t\t\n\n");
			E_Transfer_Money();
		}
		CE_Time();
		TA.Money = a * -1.00;
		if (curAccount->balance + TA.Money < 0)
		{
			int k = 0, b;
			printf("\nNot enough happy beans!!!!\n");
			while (k < 3)
			{
				printf("\n\n\n\t\t【1】 Re-enter the amount of happy beans");
				printf("\n\n\n\t\t【2】 Return to home page");
				printf("\n\n\n\t\t【0】 Exit the system\n");
				scanf("%d", &b);
				switch (b)
				{
				case 1:
					system("cls");
					E_Transfer_Money();
					k = 3;
					break;
				case 2:
					system("cls");
					E_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
		else
		{
			printf("\nSuccessful delivery of beans!!!!\n");
			curAccount->balance = curAccount->balance + TA.Money;//当前账户减去转账金额
			curAccountO->balance = curAccountO->balance - TA.Money;//对方账户加上转账金额
			CE_Transfer_Transaction();
			int k = 0, b;
			while (k < 3)
			{
				printf("\n\n\n\t\t[1] Transfer beans again");
				printf("\n\n\n\t\t[2] Return to the home page");
				printf("\n\n\n\t\t[0] Exit the system\n");
				scanf("%d", &b);
				switch (b)
				{
				case 1:
					system("cls");
					E_Transfer_Money();
					k = 3;
					break;
				case 2:
					system("cls");
					E_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
	}
	else
	{
		int b, k = 0;
		printf("\nThe account of the other party does not exist!!!!");
		while (k < 3)
		{
			printf("\n\n\n\t\t【1】 Re-enter the other party's account number");
			printf("\n\n\n\t\t【2】 Return to home page");
			printf("\n\n\n\t\t【0】 Exit the system\n\n");
			scanf("%d", &b);
			switch (b)
			{
			case 1:
				system("cls");
				E_Transfer_Money();
				k = 3;
				break;
			case 2:
				system("cls");
				E_Main_Menu();
				k = 3;
				break;
			case 0:
				k = 3;
				CE_Save_User_Data();
				exit(0);
			default:
				k++;
				break;
			}
		}
	}
}

//查询业务页
void C_Enquiry()
{

	int a, k = 0;
	while (k < 3)
	{
			printf("\n【查询】\t\t\t\t\t\t\t\t\t%s", tmp1);
			printf("\n==================================================================================================\n");
			printf("\n\n\t\t【1】账户余额查询\n");
			printf("\n\n\t\t【2】个人信息查询\n");
			printf("\n\n\t\t【3】交易记录查询\n");
			printf("\n\n\t\t【4】返回主页\n");
			printf("\n\n\t\t【0】退出系统\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			C_Enquiry_balance();
			k = 3;
			break;
		case 2:
			system("cls");
			C_Enquiry_Information();
			k = 3;
			break;
		case 3:
			system("cls");
			C_Enquiry_Record();
			k = 3;
			break;
		case 4:
			system("cls");
			C_Main_Menu();
			k = 3;
			break;
		case 0:
			system("cls");
			k = 3;
			CE_Save_User_Data();
			exit(0);
		default:
			k++;
			break;
		}
	}
}

//查询业务页
void E_Enquiry()
{

	int a, k = 0;
	while (k < 3)
	{
		printf("\n【Inquire】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\t\t【1】Account balance inquiry\n");
		printf("\n\n\t\t【2】Personal information query\n");
		printf("\n\n\t\t【3】Transaction record query\n");
		printf("\n\n\t\t【4】Return to homepage\n");
		printf("\n\n\t\t【0】Exit the system\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			E_Enquiry_balance();
			k = 3;
			break;
		case 2:
			system("cls");
			E_Enquiry_Information();
			k = 3;
			break;
		case 3:
			system("cls");
			E_Enquiry_Record();
			k = 3;
			break;
		case 4:
			system("cls");
			E_Main_Menu();
			k = 3;
			break;
		case 0:
			system("cls");
			k = 3;
			CE_Save_User_Data();
			exit(0);
		default:
			k++;
			break;
		}
	}
}

//账户余额查询
void C_Enquiry_balance()
{
	printf("\n【账户余额查询】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	CE_Time();
	printf("\n\t您的余额为：\n\t\t%.2f\n", curAccount->balance);
	printf("\n");
	int a, k = 0;
	while (k < 3)
	{
		printf("\n\n\t\t【1】返回上一页\n");
		printf("\n\n\t\t【2】返回主页\n");
		printf("\n\n\t\t【0】退出系统\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			C_Enquiry();
			k = 3;
			break;
		case 2:
			system("cls");
			C_Main_Menu();
			k = 3;
			break;
		case 0:
			k = 3;
			CE_Save_User_Data();
			exit(0);
		default:
			k++;
			break;
		}
	}
}

//账户余额查询
void E_Enquiry_balance()
{
	printf("\n【Account balance query】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	CE_Time();
	printf("\n\tYour Happy Beans also include:\n\t\t%.2f\n", curAccount->balance);
	printf("\n");
	int a, k = 0;
	while (k < 3)
	{
		printf("\n\n\t\t【1】Return to the previous page\n");
		printf("\n\n\t\t【2】Return to homepage\n");
		printf("\n\n\t\t【0】Exit the system\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			E_Enquiry();
			k = 3;
			break;
		case 2:
			system("cls");
			E_Main_Menu();
			k = 3;
			break;
		case 0:
			k = 3;
			CE_Save_User_Data();
			exit(0);
		default:
			k++;
			break;
		}
	}
}

//个人信息查询
void C_Enquiry_Information()
{
	printf("\n【个人信息查询】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	printf("\n\t姓名：\n\t\t%s\n", curAccount->name);
	printf("\n\t账号：\n\t\t%s\n", curAccount->account_number);
	printf("\n\t预留手机号：\n\t\t%s\n\n", curAccount->phone_number);
	int a, k = 0;
	while (k < 3)
	{
		printf("\n\n\t\t\t【1】返回上一页\n");
		printf("\n\n\t\t\t【2】返回主页\n");
		printf("\n\n\t\t\t【0】退出系统\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			C_Enquiry();
			k = 3;
			break;
		case 2:
			system("cls");
			C_Main_Menu();
			k = 3;
			break;
		case 0:
			k = 3;
			CE_Save_User_Data();
			exit(0);
		default:
			k++;
			break;
		}
	}
}

//个人信息查询
void E_Enquiry_Information()
{
	printf("\n【Personal information inquiry】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	printf("\n\tName：\n\t\t%s\n", curAccount->name);
	printf("\n\tAccount number：\n\t\t%s\n", curAccount->account_number);
	printf("\n\tReserved phone number：\n\t\t%s\n\n", curAccount->phone_number);
	int a, k = 0;
	while (k < 3)
	{
		printf("\n\n\t\t\t【1】Return to the previous page\n");
		printf("\n\n\t\t\t【2】Return to homepage\n");
		printf("\n\n\t\t\t【0】Exit the system\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			E_Enquiry();
			k = 3;
			break;
		case 2:
			system("cls");
			E_Main_Menu();
			k = 3;
			break;
		case 0:
			k = 3;
			CE_Save_User_Data();
			exit(0);
		default:
			k++;
			break;
		}
	}
}

//交易记录查询
void C_Enquiry_Record()
{
	printf("\n【交易记录查询】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	CE_Print_Transaction();
	int a, k = 0;
	while (k < 3)
	{
		printf("\n\n\t\t【1】返回上一页\n");
		printf("\n\n\t\t【2】返回主页\n");
		printf("\n\n\t\t【0】退出系统\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			C_Enquiry();
			k = 3;
			break;
		case 2:
			system("cls");
			C_Main_Menu();
			k = 3;
			break;
		case 0:
			k = 3;
			CE_Save_User_Data();
			exit(0);
		default:
			k++;
			break;
		}
	}
}

//交易记录查询
void E_Enquiry_Record()
{
	printf("\n【Transaction record query】\t\t\t\t\t\t\t\t\t%s", tmp1);
	printf("\n==================================================================================================\n");
	CE_Print_Transaction();
	int a, k = 0;
	while (k < 3)
	{
		printf("\n\n\t\t【1】Return to the previous page\n");
		printf("\n\n\t\t【2】Return to homepage\n");
		printf("\n\n\t\t【0】Exit the system\n");
		scanf("%d", &a);
		switch (a)
		{
		case 1:
			system("cls");
			C_Enquiry();
			k = 3;
			break;
		case 2:
			system("cls");
			C_Main_Menu();
			k = 3;
			break;
		case 0:
			k = 3;
			CE_Save_User_Data();
			exit(0);
		default:
			k++;
			break;
		}
	}
}

//修改信息选项页
void C_Change_Information()
{
	system("cls");
	int a, k2 = 1;
	while (k2)
	{
		printf("\n【修改个人资料】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\t\t【1】修改用户名称\n");
		printf("\n\n\t\t【2】修改账户号码\n");
		printf("\n\n\t\t【3】修改联系电话\n");
		printf("\n\n\t\t【4】修改账户密码\n");
		printf("\n\n\t\t【9】返回上一页\n");
		printf("\n\n\t\t【0】退出系统\n");
		scanf("%d", &a);

		switch (a)
		{
		case 1:
			C_Change_User();
			k2 = 0;
			break;
		case 2:
			C_Change_AccountNum();
			k2 = 0;
			break;
		case 3:
			C_Change_phone();
			k2 = 0;
			break;
		case 4:
			C_Change_Password();
			k2 = 0;
			break;
		case 9:
			C_Main_Menu();
			k2 = 0;
			break;
		case 0:
			CE_Save_User_Data();
			exit(0);
		default:
			system("cls");
			printf("输入错误！\t请按照提示重新输入：\n");
			break;
		}
	}
}

//修改信息选项页
void E_Change_Information()
{
	system("cls");
	int a, k2 = 1;
	while (k2)
	{

		printf("\n【Modify personal information】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\t\t【1】Modify user name\n");
		printf("\n\n\t\t【2】Modify account number\n");
		printf("\n\n\t\t【3】Modify contact number\n");
		printf("\n\n\t\t【4】Change account password\n");
		printf("\n\n\t\t【9】Go back to the last page\n");
		printf("\n\n\t\t【0】Exit system\n");
	}
	scanf("%d", &a);

	switch (a)
	{
	case 1:
		E_Change_User();
		k2 = 0;
		break;
	case 2:
		E_Change_AccountNum();
		k2 = 0;
		break;
	case 3:
		E_Change_phone();
		k2 = 0;
		break;
	case 4:
		E_Change_Password();
		k2 = 0;
		break;
	case 9:
		E_Main_Menu();
		k2 = 0;
		break;
	case 0:
		CE_Save_User_Data();
		exit(0);
	default:
		system("cls");
		printf("input error! \tPlease re-enter as prompted:\n");
		break;
	}
}

//修改用户
void C_Change_User()
{
	int a, k = 0;
	char Password[100];
	while (k < 3)
	{
		printf("\n【修改用户名称】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\t>>请输入密码：\n");
		scanf("%s", Password);
		system("cls");
		printf("\n【修改用户名称】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		if (strcmp(Password, curAccount->password) == 0)
		{
			printf("\n\n\t>>请输入新用户名称:\n");
			scanf("%s", curAccount->name);
			printf("\n【修改用户名称】\t\t\t\t\t\t\t\t\t%s", tmp1);
			printf("\n==================================================================================================\n");
			printf("\n\n\t用户名称修改成功！\n");
			CE_Save_User_Data();
			k = 3;

			int k2 = 0;
			while (k2 < 3)
			{
				printf("\n\n\t\t【1】返回上一页\n");
				printf("\n\n\t\t【2】返回主页\n");
				printf("\n\n\t\t【0】退出系统\n");
				scanf("%d", &a);
				switch (a)
				{
				case 1:
					system("cls");
					C_Change_Information();
					k = 3;
					break;
				case 2:
					system("cls");
					C_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
		else
		{
			k++;
			printf("密码输入错误! \t%d\t请按照提示重新输入：\n", 3 - k);
		}
	}
}

//修改用户
void E_Change_User()
{
	int a, k = 0;
	char Password[100];
	while (k < 3)
	{
		printf("\n【Modify user name】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\t>>Please enter password:\n");
		scanf("%s", Password);
		system("cls");
		printf("\n【Modify user name】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		if (strcmp(Password, curAccount->password) == 0)
		{
			printf("\n\n\t>>Please enter a new user name:\n");
			scanf("%s", curAccount->name);
			printf("\n【Modify user name】\t\t\t\t\t\t\t\t\t%s", tmp1);
			printf("\n==================================================================================================\n");
			printf("\n\n\tUser name modified successfully!\n");
			CE_Save_User_Data();
			k = 3;

			int k2 = 0;
			while (k2 < 3)
			{
				printf("\n\n\t\t【1】Return to the previous page\n");
				printf("\n\n\t\t【2】Return to homepage\n");
				printf("\n\n\t\t【0】Exit the system\n");
				scanf("%d", &a);
				switch (a)
				{
				case 1:
					system("cls");
					E_Change_Information();
					k = 3;
					break;
				case 2:
					system("cls");
					E_Main_Menu();
					k = 3;
					break;
				case 0:
					k = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k++;
					break;
				}
			}
		}
		else
		{
			k++;
			printf("Wrong password entered! \t%d\tPlease re-enter as prompted:\n", 3 - k);
		}
	}
}

//修改账户
void C_Change_AccountNum()
{
	system("cls");
	int a, k = 0;
	char Password[100];
	while (k < 3)
	{
		printf("\n【修改账户号码】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\n\t>>请输入密码：\n");
			printf("\n【Modify account number】\n======================================== ==\n\n\n\t>>Please enter password:\n");
		scanf("%s", Password);
		system("cls");
		printf("\n【修改账户号码】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		if (strcmp(Password, curAccount->password) == 0)
		{
			printf("\n\n\t>>请输入新账户号码:\n");
			scanf("%s", curAccount->account_number);
			system("cls");
			printf("\n【修改账户号码】\t\t\t\t\t\t\t\t\t%s", tmp1);
			printf("\n==================================================================================================\n");
			printf("\n\n\t账户号码修改成功！\n");
			CE_Save_User_Data();
			k = 3;

			int k2 = 0;
			while (k2 < 3)
			{
				printf("\n\n\t\t【1】返回上一页\n");
				printf("\n\n\t\t【2】返回主页\n");
				printf("\n\n\t\t【0】退出系统\n");
				scanf("%d", &a);
				switch (a)
				{
				case 1:
					system("cls");
					C_Change_Information();
					k2 = 3;
					break;
				case 2:
					system("cls");
					C_Main_Menu();
					k2 = 3;
					break;
				case 0:
					k2 = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k2++;
					break;
				}
			}
		}
		else
		{
			k++;
			printf("密码输入错误! \t%d\t请按照提示重新输入：\n", 3 - k);
		}
	}
}

//修改账户
void E_Change_AccountNum()
{
	system("cls");
	int a, k = 0;
	char Password[100];
	while (k < 3)
	{
		printf("\n【Modify account number】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\n\t>>Please enter password:\n");
		scanf("%s", Password);
		system("cls");
		printf("\n【Modify account number】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		if (strcmp(Password, curAccount->password) == 0)
		{
			printf("\n\n\t>>Please enter new account number:\n");
			scanf("%s", curAccount->account_number);
			system("cls");
			printf("\n【Modify account number】\t\t\t\t\t\t\t\t\t%s", tmp1);
			printf("\n==================================================================================================\n");
			printf("\n\n\tThe account number has been modified successfully!\n");
			CE_Save_User_Data();
			k = 3;

			int k2 = 0;
			while (k2 < 3)
			{
				printf("\n\n\t\t【1】Return to the previous page\n");
				printf("\n\n\t\t【2】Return to homepage\n");
				printf("\n\n\t\t【0】Exit the system\n");
				scanf("%d", &a);
				switch (a)
				{
				case 1:
					system("cls");
					E_Change_Information();
					k2 = 3;
					break;
				case 2:
					system("cls");
					E_Main_Menu();
					k2 = 3;
					break;
				case 0:
					k2 = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k2++;
					break;
				}
			}
		}
		else
		{
			k++;
			printf("Wrong password entered! \t%d\tPlease re-enter as prompted:\n", 3 - k);
		}
	}
}

//修改联系电话
void C_Change_phone()
{
	system("cls");
	int a, k = 0;
	char Password[100];
	while (k < 3)
	{
		printf("\n【修改联系电话】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\t>>请输入密码：\n");
		scanf("%s", Password);
		system("cls");
		printf("\n【修改联系电话】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		if (strcmp(Password, curAccount->password) == 0)
		{
			printf("\n\n\t>>请输入新联系电话号码:\n");
			scanf("%s", curAccount->phone_number);
			system("cls");
			printf("\n【修改联系电话】\t\t\t\t\t\t\t\t\t%s", tmp1);
			printf("\n==================================================================================================\n");
			printf("\n\n\t联系电话修改成功！\n");
			CE_Save_User_Data();
			k = 3;

			int k2 = 0;
			while (k2 < 3)
			{
				printf("\n\n\t\t【1】返回上一页\n");
				printf("\n\n\t\t【2】返回主页\n");
				printf("\n\n\t\t【0】退出系统\n");
				scanf("%d", &a);
				switch (a)
				{
				case 1:
					system("cls");
					C_Change_Information();
					k2 = 3;
					break;
				case 2:
					system("cls");
					C_Main_Menu();
					k2 = 3;
					break;
				case 0:
					k2 = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k2++;
					break;
				}
			}
		}
		else
		{
			k++;
			printf("密码输入错误! \t%d\t请按照提示重新输入：\n", 3 - k);
		}
	}
}

//修改联系电话
void E_Change_phone()
{
	system("cls");
	int a, k = 0;
	char Password[100];
	while (k < 3)
	{
		printf("\n[Modify contact number]\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\n\t>>Please enter your password:\n");
		scanf("%s", Password);
		system("cls");
		printf("\n[Modify contact number]\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		if (strcmp(Password, curAccount->password) == 0)
		{
			printf("\n\n\t>>Please enter a new contact phone number:\n");
			scanf("%s", curAccount->phone_number);
			system("cls");
			printf("\n[Modify contact number]\t\t\t\t\t\t\t\t\t%s", tmp1);
			printf("\n==================================================================================================\n");
			printf("\n\n\tContact phone number modified successfully!\n");
			CE_Save_User_Data();
			k = 3;

			int k2 = 0;
			while (k2 < 3)
			{
				printf("\n\n\t\t【1】Go back to the last page\n");
				printf("\n\n\t\t【2】Return to home page\n");
				printf("\n\n\t\t【0】Exit system\n");
				scanf("%d", &a);
				switch (a)
				{
				case 1:
					system("cls");
					E_Change_Information();
					k2 = 3;
					break;
				case 2:
					system("cls");
					E_Main_Menu();
					k2 = 3;
					break;
				case 0:
					k2 = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k2++;
					break;
				}
			}
		}
		else
		{
			k++;
			printf("\n\n\tPassword entered incorrectly! \t%d\tPlease re-enter as prompted:\n", 3 - k);
		}
	}
}

//修改密码
void C_Change_Password()
{
	system("cls");
	int a, k = 0;
	char Password[100];
	while (k < 3)
	{
		printf("\n【修改账户密码】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\n\t>>请输入密码：\n");
		scanf("%s", Password);
		system("cls");
		printf("\n【修改账户密码】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		if (strcmp(Password, curAccount->password) == 0)
		{
			printf("\n\n\t>>请输入新密码:\n");
			scanf("%s", curAccount->password);
			system("cls");
			printf("\n【修改账户密码】\t\t\t\t\t\t\t\t\t%s", tmp1);
			printf("\n==================================================================================================\n");
			CE_Save_User_Data();
			k = 3;

			int k2 = 0;
			while (k2 < 3)
			{
				printf("\n\n\t\t【1】返回上一页\n");
				printf("\n\n\t\t【2】返回主页\n");
				printf("\n\n\t\t【0】退出系统\n");
				scanf("%d", &a);
				switch (a)
				{
				case 1:
					system("cls");
					C_Change_Information();
					k2 = 3;
					break;
				case 2:
					system("cls");
					C_Main_Menu();
					k2 = 3;
					break;
				case 0:
					k2 = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k2++;
					break;
				}
			}
		}
		else
		{
			k++;
			printf("\n\n\t密码输入错误! \t%d\t请按照提示重新输入：\n", 3 - k);
		}
	}
}

//修改密码
void E_Change_Password()
{
	system("cls");
	int a, k = 0;
	char Password[100];
	while (k < 3)
	{
		printf("\n【Change account password】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		printf("\n\n\n\t>>Please enter your password:\n");
		scanf("%s", Password);
		system("cls");
		printf("\n【Change account password】\t\t\t\t\t\t\t\t\t%s", tmp1);
		printf("\n==================================================================================================\n");
		if (strcmp(Password, curAccount->password) == 0)
		{
			printf("\n\n\t>>Please enter a new password:\n");
			scanf("%s", curAccount->password);
			system("cls");
			printf("\n【Change account password】\t\t\t\t\t\t\t\t\t%s", tmp1);
			printf("\n==================================================================================================\n");
			CE_Save_User_Data();
			k = 3;

			int k2 = 0;
			while (k2 < 3)
			{
				printf("\n\n\t\t【1】Go back to the last page\n");
				printf("\n\n\t\t【2】Return to home page\n");
				printf("\n\n\t\t【0】Exit system\n");
				scanf("%d", &a);
				switch (a)
				{
				case 1:
					system("cls");
					E_Change_Information();
					k2 = 3;
					break;
				case 2:
					system("cls");
					E_Main_Menu();
					k2 = 3;
					break;
				case 0:
					k2 = 3;
					CE_Save_User_Data();
					exit(0);
				default:
					k2++;
					break;
				}
			}
		}
		else
		{
			k++;
			printf("\n\n\t密码输入错误! \t%d\t请按照提示重新输入：\n", 3 - k);
		}
	}
}

//开始界面
void start()
{
	int n1, k = 1;
	while (k)
	{
		printf("\n\n\t\t请选择语言：\t");
		printf("Please choose language：\n");
		printf("\n\n\t\t\t【1】中文\n");
		printf("\n\n\t\t\t【2】English\n");
		printf("\n\n\t\t\t【0】退出 Exit\n\n\n\t\t\t");
		printf("\n\n\t\t\t您的选择/Your choice：___\b\b\b");
		scanf("%d", &Language);

		switch (Language)
		{
		case 1:
			system("cls");
			printf("\n\n\t\t\t【1】用户登录\n");
			printf("\n\n\t\t\t【2】用户注册\n");
			printf("\n\n\t\t\t【其它】返回上一页\n");
			printf("\n\n\t\t\t您的选择：___\b\b\b");
			break;
		case 2:
			system("cls");
			printf("\n\n\t\t\t【1】Sign in\n");
			printf("\n\n\t\t\t【2】Sign up\n");
			printf("\n\n\t\t\t【Others】Back to previous page\n");
			printf("\n\n\t\t\tYour choice：___\b\b\b");
			break;
		case 0:
			exit(0);
		default:
			k = 1;
			printf("\n\n\t\t输入有误，请按照提示再次输入!\t\t");
			printf("Error,please follow the prompts to enter again!\n\n");
			break;
		}
		scanf("%d", &n1);
		k = 0;
		if (n1 == 1)
		{
			system("cls");
			C_Sign_In();//中文注册函数
		}
		else if (n1 == 2)
		{
			system("cls");
			C_Sign_Up();
		}
		else
			k = 1;
	}
}

int main()
{
	CE_Time();
	CE_Initialize_User_Data();
	CE_Initialize_Transaction_Data();
	start();

	//printLinkedList();
	CE_Save_User_Data();
	CE_Save_Transaction_Data();
	return 0;
}

