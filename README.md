#include<stdio.h>
#include<string.h>
#define MAX_PRODUCTS 10
//structure fo product
typedef struct 
{
    char name[30];
    int id;
    float price;
    int stock;
}product;
product products[MAX_PRODUCTS];
int product_no=0;
int registeredRole;
char registerName[30];
char registeredPassword[30];

void registerUser();
int login();
void adminMenu();
void shopStaffmenu();
void addProduct();
void viewProduct();
void searchProduct();
void restockProduct();
void deleteProduct();
void saveProductData();

int main(int argc, char const *argv[])
{   
    
    printf("\n\nWelcome to the Super Shop Management System\n");
    printf("============================================\n");
    int choice;
    
    do{
        printf("\t\tMain Menu\n");
        printf("\t    ================\n");
        printf("\n1.Register New User\n2.Log in\n");
        printf("3.Exit\n");
        printf("\nEnter your choice: ");
        scanf("%d",&choice);

        switch(choice){
            case 1: 
                registerUser();
                break;
            case 2: 
                {
                int role = login();
                if(role == 1){
                    adminMenu();
                    break;
                }else{
                    shopStaffmenu();
                    break;
                }
                }
                
            case 3:
                printf("\nExiting....  Thank you\n\n");
                break;

            default:
                printf("Invalid choice. Try again.");
        }

        }while(choice!=3);


    return 0;
}

//registering user

void registerUser(){

    printf("\nEnter user name: ");
    scanf("%s",registerName);
    printf("Enter password: ");
    scanf("%s",registeredPassword);
    printf("Enter role (1.Admin  2.Shopstaff): ");
    scanf("%d",&registeredRole);
    if(registeredRole!=1&&registeredRole!=2){
        printf("Invaild role. Try again.");
        return;
    }else{
    printf("\nRegister successful. Please login.\n\n");
    }
}

//login fucntion

int login(){
    char username[30],password[30];
    int role;
    printf("\nEnter Name: ");
    scanf("%s",username);
    printf("Enter Password: ");
    scanf("%s",password);
    printf("Enter role (1.Admin  2.Shopstaff): ");
    scanf("%d",&role);
    if(strcmp(registerName,username)==0&&strcmp(registeredPassword,password)==0&&(registeredRole==role)){
        printf("Login successful.\n");
        return role;
    }else{
        printf("\nIvalid username or password.\n");
        return -1;
    }
}


//admin manu
void adminMenu(){
    int choice;
    do{  
    printf("\n\t\tAdmin Menu");
    printf("\n\t     ===============");
    printf("\n\n1.Add new product\t2.Search product\n");
    printf("3.Delete product\t4.View all product\n");
    printf("5.Restock product\t6.Save data and exit\n");
    printf("7.Logout!\n");
    printf("\n\nEnter your choice: ");
    scanf("%d",&choice);

    switch (choice)
    {
    case 1:
        addProduct();
        break;
    case 2:
        searchProduct();
        break;
    case 3:
        deleteProduct();
        break;
    case 4:
        viewProduct();
        break;
    case 5:
        restockProduct();
        break;
    case 6:
        saveProductData();
        break;
    case 7:
        printf("\nLogging Out....\n");
        break;
    default:
        printf("Invalid Choice");
        break;
    }

    }while(choice!=7);
}

//stasff menu
void shopStaffmenu(){
    int choice;
    do{  
    printf("\n\t\tStaff Menu");
    printf("\n\t     ===============");
    printf("\n\n1.Search product\t");
    printf("2.View all product\n");
    printf("3.Logout!\n");
    printf("\n\nEnter your choice: ");
    scanf("%d",&choice);
    switch (choice)
    {
    case 1:
        searchProduct();
        break;
    case 2:
        viewProduct();
        break;
    case 3:
        printf("Logging out....\n");
        break;
    default:
        printf("Invalid choice.\n");
        break;
    }
    }
    while(choice!=3);
}

//adding product
void addProduct(){
    if(product_no<MAX_PRODUCTS){
        printf("\nEnter product ID: ");
        scanf("%d",&products[product_no].id);
        printf("Enter product Name: ");
        scanf("%s",products[product_no].name);
        printf("Enter product Stock level: ");
        scanf("%d",&products[product_no].stock);
        printf("Enter product Price: ");
        scanf("%f",&products[product_no].price);

        printf("\nProduct added successfully\n");
        product_no++;
    }else{
        printf("Maximum product limit reached.");
    }
}
//viewing product
void viewProduct(){
    printf("\n\n\t\tProduct List\n");
    printf("\nId\tName\tStock\tPrice\n");
    printf("----------------------------------------------\n");
    
    if(product_no==0){
        printf("\nNo product available\n");
    }else{
        for(int i=0;i<product_no;i++){
            
        printf("%d\t%s\t%d\t%0.2f\n",products[i].id,products[i].name,products[i].stock,products[i].price);
        }
        
    }
}
//searching for product
void searchProduct(){
    int searchid,choice,found=0;
    char searchname[30];
    printf("Search By: \n1.Product Id\n2.Product Name\nEnter your choice: ");
    scanf("%d",&choice);
    if(choice == 1){        //search by id
        printf("\nEnter product id to search: ");
        scanf("%d",&searchid);
        for(int i =0 ;i<product_no;i++){
            if(searchid==products[i].id){
                printf("\nProduct Details: \n");
                printf("Product Id: %d\n",products[i].id);
                printf("Product Name: %s\n",products[i].name);
                printf("Product Price: %0.2f\n",products[i].price);
                printf("Product Stock Level: %d\n",products[i].stock);
                found=1;
                break;
            }
        }
    }else if(choice==2){     //search by name
        printf("\nEnter product name to search: ");
        scanf("%s",searchname);
        for(int i = 0;i<product_no;i++){
            if(strcmp(searchname,products[i].name)==0){
                printf("\nProduct Details: \n");
                printf("Product Id: %d\n",products[i].id);
                printf("Product Name: %s\n",products[i].name);
                printf("Product Price: %0.2f\n",products[i].price);
                printf("Product Stock Level: %d\n",products[i].stock);
                found=1;
                break;
            }
        }
    }else{
        printf("\nInvalid choice.");
        return;
    }
    if(found==0){
        printf("\nProduct not found!");
    }
}
// restocking prodcut
void restockProduct(){
    int searchid ,addstock;
    int found=0;
    printf("\nEnter product Id to restock: ");
    scanf("%d",&searchid);
    printf("\nEnter the amount of stock: ");
    scanf("%d",&addstock);
    for(int i=0;i<product_no;i++){
        if(searchid==products[i].id){
            products[i].stock += addstock; 
            printf("Product Id: %d restocked successfully",products[i].id);
            found=1;
            break;
        }
    }
    if(found == 0){
        printf("\nProduct not found!.\n");
    }
}
    //deleting product
void deleteProduct(){
    int searchid,found=0;
    printf("\nEnter product Id to delete: ");
    scanf("%d",&searchid);
    for(int i=0;i<product_no;i++){
        if(searchid==products[i].id){
            for(int j=i;j<product_no-1;j++){
                products[j]=products[j+1];
            }
            product_no--;
            found=1;
            break;
        }
    }if(found==1){
        printf("\nProduct Id: %d deleted successfully.\n",searchid);
    }else{
        printf("\nProduct not found.\n");
    }
}
//savig data to a file
void saveProductData(){
    FILE *file;
    file = fopen("Management.txt","w");
    if(file==NULL){
        printf("\nError opening file.\n");
    }else{
        fprintf(file,"Products:\n");
        for(int i=0;i<product_no;i++){
            fprintf(file,"Id: %d\tName: %s\tPrice: %0.2f\tStock: %d\n",products[i].id,products[i].name,products[i].price,products[i].stock);
        }
        fclose(file);
        printf("\nFile saved successfully\n");
    }
}
