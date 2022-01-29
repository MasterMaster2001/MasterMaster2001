
<!-- Logo -->
<p>
  <p align=left>
     <img src="eidiaeuro.png" width=200 height=100>
    <img src="euromed.png" width=200 height=100 align= right>
  </p>
 </p>
 

<!-- PROJECT LOGO -->
<br />
<div align="center">
    <h2 align="center">Programmation orientée objet (POO) :C++</h2>
  <h3 align="center"><span style="color:red">A COVID-19 Vaccination</span></h3>
    <img src="azs (1).jpg" alt="Logo" width="800" height="360">
</div>



* * *
## Introduction:
  1. Objectif
  2. La mise en œuvre nécessaire pour ce programme.
  3. Page de connexion principale (5 boutons peuvent effectuer différentes actions)
  4. les codes et les bibliothèques nécessaires à la compilation de ce programme                                          (pour chaque fichier).
  5. Pour la page utilisateur : (bouton s'inscrire)
  6. Pour la page utilisateur : (bouton de connexion) et boîte de dialogue suivante
  7. Pour la page d'administration : (bouton s'inscrire)
  8. Pour la page d'administration : (bouton de connexion) et boîte de dialogue suivante
  9. Pour la page principale (bouton quitter)


* * *

* * *
<p>
  <p align=left>
    <h1 align="left">1.	Objectif :</h2>
     </p>
 </p>
Cette application en tant que simulation peut aider les gens à planifier facilement un vaccin. Quand il y a un nouveau vaccin sorti et ouvert au public, il y a beaucoup de gens qui Veulent l’obtenir, mais il est difficile de le planifier parce que les gens ont un temps de travail différent, aussi Le nombre de vaccins est limité au début. Cette application aidera l’utilisateur à s’inscrire, choisir les vaccins et planifier leurs rendez-vous. Pour le système d’administration, l’administrateur peut vérifier les informations de l’utilisateur et être en mesure d’enregistrer, de modifier, de mettre à jour et de supprimer toutes informations dans le système.

<p>
  <p align=left>
     <h1 align="left">2.	La mise en œuvre nécessaire pour ce programme :</h2>
      <h3 align="left">DB (Data Base) Navigateur pour SQLite :</h2>
Stockez toutes les données dont nous avons besoin pour ce programme, il est inclus deux tables (table utilisateur, table admin)
<h3 align="left"> Source file ( trois images):</h2>

  </p>
 </p>


* * *
* * *
<p>
  <p align=left>  <h1 align="left">3.	Page de connexion principale (5 boutons peuvent effectuer différentes actions) :</12></p>
 </p>



![interface_zas](https://user-images.githubusercontent.com/93833171/151639759-19b499ad-69ca-4966-95c6-39d8bef5b9bb.jpg)

* * *
<p>
  <p align=left>  <h1 align="left">4. bibliothèques nécessaires à la compilation de ce programme (pour chaque fichier):</12></p>
 </p>
 
 * * *
 <p>
  <p align=left>  <h1 align="left">A. main.cpp :</h></p>
  
  <p align=left>  <h4 align="left">
  
  ```cpp
#include "Login.h"

#include <QApplication>
#include <QTabWidget>
#include <QPushButton>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    //set the app style sheet
    QFile styleSheetFile(":/qss/Irrorater.qss");
    styleSheetFile.open(QFile::ReadOnly);
    QString styleSheet = QLatin1String(styleSheetFile.readAll());
    a.setStyleSheet(styleSheet);

    Login w;
    w.show();   // window is fefault when it built. So need be show by reset "show";
    return a.exec();  // make the program keep running, wait the event happen
}

  ```
  </h4></p>
  
   <p align=left>  <h1 align="left">B. Login.cpp:</></p>
  
  <p align=left>  <h4 align="left">
   
  ```cpp
#include "Login.h"
#include "ui_Login.h"
#include "UserDialog1.h"
#include <QMessageBox>
#include "userMain.h"
#include "user.h"
#include "user.cpp"
#include <QSqlError>
#include <QSqlQuery>
#include <QDebug>
#include <QString>
#include <QException>

Login::Login(QWidget *parent): QMainWindow(parent), ui(new Ui::Login){
    ui->setupUi(this);
    if(!connOpen()){
        ui->label->setText("Failed to open the database!");
        qDebug()<<"The database Not connected ";
    }else{
        ui->label->setText("Connected database........");
        qDebug()<<"The database connected ";
    }
}

Login::~Login(){
    delete ui;
}

// for action admin register....(admin_username,admin_password,admin_code)
void Login::on_pushButton_adminRegister_clicked(){
    Login conn;
    QString adminUsername, adminPassword, adminCode;

    adminUsername = ui->lineEdit_adminUsername->text();
    adminPassword = ui->lineEdit_adminPW->text();
    adminCode = ui->lineEdit_adminCode->text();

    if(!connOpen()){
        qDebug() <<"Failed to open the database";
        return;
    }
    connOpen();   // open the database function
    QSqlQuery qry;
    qry.prepare("insert into admin (admin_username,admin_password,admin_code) values ('"+adminUsername+"','"+adminPassword+"','"+adminCode+"')" );

    qDebug()<<qry.executedQuery();
    if(qry.exec()) {
        QMessageBox::information(this,tr("Save"),tr("Admin Data Saved!"));
        conn.connClose();
    }else{
        QMessageBox::critical(this,tr("Error"),qry.lastError().text());
    }
}

// for action admin login select 3 columns from database
void Login::on_pushButton_adminLogin_clicked(){
    int count = 0;
    //.........................................................Exception.....................................//
    try {
        QString adminUsername = ui ->lineEdit_adminUsername->text();   // login and get the text of username and password form user input.
        QString adminPassword = ui->lineEdit_adminPW->text();
        QString adminCode = ui->lineEdit_adminCode->text();
        connOpen();
        QSqlQuery qry;
        qry.prepare("select* from admin where admin_username='"+adminUsername + "'and admin_password = '"+adminPassword+"'and admin_code = '"+adminCode+"'");

        if(qry.exec()) {
            while(qry.next()){
                count++;
            }

            qDebug() << count;
            if(count==1){
                connClose();    // close the database before open next window
                this->hide();
                adminP1 = new AdminDialog1(this);
                adminP1->show();
            }else{
                throw("Error");
            }
        }
    }
    catch (...) {
        ui->label ->setText("Username or password or work code is NOT correct.");
        qDebug() << "Catch Error： Username or password or work code is NOT correct. ";
    }

}

// for action  user login select 2 columns
void Login::on_pushButton_userLogin_clicked(){
    int count = 0;
    //.........................................................Exception.....................................//
    try {
        QString userUsername = ui ->lineEdit_userUsername->text();     // login and get the text of username and password form user input.
        QString userPassword = ui->lineEdit_userPW->text();
        connOpen();                                                    // open the database function, there are debug in the function.
        QSqlQuery qry;
        qry.prepare("select* from userInfo where username='"+userUsername+ "' and password = '"+userPassword+"'" );

        if(qry.exec()) {
            while(qry.next()){
                count++;
            }
            qDebug() << count;
            if(count==1){
                ui->label ->setText("Username and password is correct.");
                connClose();
                this->hide();
                userP1 = new UserDialog1(this);
                userP1->user_name = userUsername;
                userP1->pw = userPassword;

                qDebug() << userP1->user_name + " has logged in";
                userP1->show();
            }
            else if (count == 0) {
                throw("Error");
            }
        }
    }
    catch (...) {
        ui->label ->setText("Username or password is NOT correct.");
        qDebug() << "Catch Error： Username or password is NOT correct. ";
    }
}


void Login::on_pushButton_userRegister_clicked(){
    Login conn;
    User curUser;
    curUser.username = ui->lineEdit_userUsername->text();
    curUser.password = ui->lineEdit_userPW->text();

    if(!connOpen()){
        qDebug() <<"Failed to open the database";
        return;
    }
    connOpen();
    QSqlQuery qry;
    qry.prepare("insert into userInfo (username,password) values ('"+curUser.username+"','"+curUser.password+"')" );
    qDebug()<<qry.executedQuery();

    if(qry.exec()) {
        QMessageBox::information(this,tr("Save"),tr("User Data Saved!"));
        conn.connClose();
    }else{
        QMessageBox::critical(this,tr("Error"),qry.lastError().text());
    }
}


void Login::on_pushButton_loginQuit_clicked(){
    QMessageBox::StandardButton reply = QMessageBox::question(this,"My Title","Are you sure to quit the login page? ", QMessageBox::Yes| QMessageBox::No);
    if(reply == QMessageBox::Yes){
        QApplication::quit();
    }else{
        qDebug() << "Answer 'NO' Button is clicked";
    }
}

   ```
  </h4></p>
<p align=left>  <h1 align="left">B. Login.h:</12></p>
  
  <p align=left>  <h4 align="left">
  
 ```cpp
#ifndef LOGIN_H
#define LOGIN_H
#include <QMainWindow>
#include "UserDialog1.h"
#include "AdminDialog1.h"
#include <QtSql>
#include <QtDebug>
#include <QFileInfo>
#include <QtSql>
#include <QtDebug>
#include <QFileInfo>

QT_BEGIN_NAMESPACE
namespace Ui { class Login; }
QT_END_NAMESPACE

class Login : public QMainWindow
{
    Q_OBJECT

public:
    QSqlDatabase mydb;

    void connClose(){
        mydb.close();
        mydb.removeDatabase(QSqlDatabase::defaultConnection);
    }

    bool connOpen(){
        mydb = QSqlDatabase::addDatabase("QSQLITE");
        mydb.setDatabaseName("C:/Users/azert/Downloads/Covid-19-Vaccine-Application-master (1)/Covid-19-Vaccine-Application-master/VaccineSchedule/mydb.db");
        if(!mydb.open()){
            qDebug()<<("Failed to open the database!");
            return false;
        }else{
            qDebug()<<("Connected database........");
            return true;
        }
    }

public:
    Login(QWidget *parent = nullptr);
    ~Login();

private slots:
    void on_pushButton_userLogin_clicked();
    void on_pushButton_adminLogin_clicked();
    void on_pushButton_userRegister_clicked();
    void on_pushButton_adminRegister_clicked();
    void on_pushButton_loginQuit_clicked();

private:
    Ui::Login *ui;
    UserDialog1 *userP1;
    AdminDialog1 *adminP1;
};
#endif // LOGIN_H

```
  </h4></p>
<p align=left>  <h1 align="left">C. User.cpp:</12></p>
  
  <p align=left>  <h4 align="left">
  
 ```cpp
  
#include "user.h"
#include "Login.h"
#include <iostream>
#include <QString>
using namespace std;

// Default constructor
User::User() {
    QString username = NULL,
            password = NULL,
            insurance = NULL,
            firstName = NULL,
            lastName = NULL,
            age = NULL,
            vaccine = NULL,
            shot = NULL,
            date = NULL,
            time = NULL;
}

// constructor for first login page, two variables
User::User(QString un, QString pw){
    this->username = un;
    this->password = pw;
    this->insurance = "0";
    this->firstName = "NULL",
            this->lastName = "NULL",
            this->age = "0",
            this->vaccine = "NULL",
            this->shot = "0",
            this->date = "xx-xx-xxxx",
            this->time = "0:00 - 0:00";
}

User::User(QString un, QString pw, QString insur, QString fn, QString ln, QString age, QString vacc, QString shot, QString date, QString time){
    this->username = un;
    this->password = pw;
    this->insurance = insur;
    this->firstName = fn;
    this->lastName = ln;
    this->age = age;
    this->vaccine = vacc;
    this->shot = shot;
    this->date = date;
    this->time = time;
}
  
```
</h4></p>
  

 <p align=left>  <h1 align="left">C. User.h:</12></p>
  
  <p align=left>  <h4 align="left">
  
 ```cpp
 #pragma once

#ifndef USER_H
#define USER_H
#include <iostream>
#include <QString>
using namespace std;

class User{
private:

public:
    QString username,password,insurance,firstName,lastName,age,vaccine,shot,date,time; //private will be better

    // constructors
    User();
    User(QString un, QString pw);
    User(QString un, QString pw, QString insur, QString fn, QString ln, QString age, QString vacc, QString shot, QString date, QString time);


    // ******* Mutators
    void setUsername(QString un);
    void setPassword(QString pw);
    void setInsurance(QString insur);
    void setFirstName(QString fn);
    void setLastName(QString ln);
    void setAge(QString age);
    void setVaccine(QString vacc);
    void setShot(QString shot);
    void setDate(QString date);
    void setTime(QString time);

    // ********* Accessors
    QString getUsername();
    QString getPassword();
    QString getInsurance();
    QString getFirstName();
    QString getLastName();
    QString getAge();
    QString getVaccine();
    QString getShot();
    QString getDate();
    QString getTime();

};

#endif // USER_H
 
 ```
  </h4></p>
  
<p align=left>  <h1 align="left">D. UserDialog1.cpp:</12></p>
 
 <p align=left>  <h4 align="left">
  
 ```cpp
#include <QDialog>
#include "ui_UserDialog1.h"
#include "UserDialog1.h"
#include "UserDL2.h"
#include <QPixmap>
#include <QMessageBox>
#include <QDebug>
#include "Login.h"
#include "user.h"

UserDialog1::UserDialog1(QWidget *parent) : QDialog(parent),ui(new Ui::UserDialog1){
    ui->setupUi(this);
    int w = ui->label_chooseVaccine->width();
    int h = ui->label_chooseVaccine->height();
    QPixmap pic1(":/img/vaccinePic1.png");
    ui->label_chooseVaccine->setPixmap(pic1.scaled(w,h,Qt::KeepAspectRatio));

    Login conn;     // create a new object
    if(!conn.connOpen()){
        ui->label_userP1->setText("Failed to open the database!");
        qDebug()<<"The database Not connected with UserDialog1 page.";
    }else{
        ui->label_userP1->setText("Connected database........");
        qDebug()<<"The database connected with UsserDialog1 page.";
    }
}
// google variables for save the data from different actions.
QString age;
QString vaccine;
QString shot;
QString date;
QString tim;

UserDialog1::~UserDialog1(){
    delete ui;
}


void UserDialog1::on_pushButton_userNextPage_clicked(){
    Login conn;
    User user;
    user.username = user_name;
    user.password = pw;
    user.insurance = ui->lineEdit_insurNum->text();
    user.firstName = ui->lineEdit_fName->text();
    user.lastName = ui->lineEdit_lName->text();
    user.age = age;
    user.vaccine = vaccine;
    user.shot = shot;
    user.date = ui->dateEdit->text();
    user.time = tim;

    if(!conn.connOpen()){
        qDebug() <<"Failed to open the database";
        return;
    }
    conn.connOpen();
    QSqlQuery qry;

    qry.prepare("update userInfo set username='"+user.username+"',password='"+user.password+"',insurance='"+user.insurance+"',firstName='"+user.firstName+"',lastName='"+user.lastName+"'"
             ",age='"+user.age+"',vaccine='"+user.vaccine+"',shot='"+user.shot+"',date='"+user.date+"',time='"+user.time+"' where username='"+user.username+"'");

    qDebug()<<qry.executedQuery();
    if(qry.exec()) {
        QMessageBox::information(this,tr("Save"),tr("Data Saved!"));
        conn.connClose();
    }else{
        QMessageBox::critical(this,tr("Error"),qry.lastError().text());
    }

    window()->hide();

    qDebug()<< user.username + " saved infomation: ";
    qDebug()<< user.username + " , " + user.password + " , " + user.insurance + " , " + user.firstName + " , " + user.lastName +
               " , " + user.age + " , " + user.vaccine + " , " + user.shot + " , " + user.date + " , " + user.time;

    //............................Writing / Creating into a new file in Qt.................................//

    QString filename="UserInfomation.txt";
    QFile file( filename );
    if ( file.open(QIODevice::ReadWrite) )
    {
        QTextStream stream( &file );

        stream<<"Hi, I am writting the user's vaccine infomation form My Qt to this file : " << Qt::endl ;

        stream<<"Username: " +user.username <<Qt::endl;
        stream <<"password: " + user.password <<Qt::endl;
        stream <<"insurance: " + user.insurance <<Qt::endl;
        stream <<"First Name: " + user.firstName <<Qt::endl;
        stream <<"Last Name: " + user.lastName <<Qt::endl;
        stream <<"Age: " + user.age <<Qt::endl;
        stream <<"Vaccine: " + user.vaccine <<Qt::endl;
        stream <<"Shot: " + user.shot <<Qt::endl;
        stream <<"Date: " + user.date <<Qt::endl;
        stream <<"Time: " + user.time <<Qt::endl;
    }

    //................................Writing / Creating into a new file in Qt..............................//

    // qry.prepare("insert into userInfo (username,password,insurance,firstName,lastName,age,vaccine,shot,date,time) "
    //  "values ('"+user.username+"','"+user.password+"','"+user.insurance+"','"+user.firstName+"','"+user.lastName+"','"+user.age+"','"+user.vaccine+"','"+user.shot+"','"+user.date+"','"+user.time+"' )" );

    /* qry.prepare("INSERT INTO userInfo(username,password,insurance,firstName,lastName,age,vaccine,shot(1st, 2nd),date,time) "
               "VALUES(?, ?, ?, ?, ?, ?, ?, ?, ?, ?)");
    qry.addBindValue(user.username);
    qry.addBindValue(user.password);
    qry.addBindValue(user.insurance);
    qry.addBindValue(user.firstName);
    qry.addBindValue(user.lastName);
    qry.addBindValue(user.age);
    qry.addBindValue(user.vaccine);
    qry.addBindValue(user.shot);
    qry.addBindValue(user.date);
    qry.addBindValue(user.time);
    qry.exec();
  */
}


// for Radio Buttom, choose vaccine, user can only choose one kind of vaccine
void UserDialog1::on_radioButton_Jonson_clicked(){
    vaccine = "Jonson & Jonson";
}

void UserDialog1::on_radioButton_moderna_clicked(){
    vaccine = "Moderna";
}

void UserDialog1::on_radioButton_pfizer_clicked(){
    vaccine = "Pfizer-BioNTech";
}


// for Radio Buttom, choose age, only can choose one
void UserDialog1::on_radioButton_under18_clicked(){
    age = "Under 18";
}

void UserDialog1::on_radioButton_over18_clicked(){
    age = "Between 18~50";
}

void UserDialog1::on_radioButton_over50_clicked(){
    age = "Over 50";
}

void UserDialog1::on_radioButton_over65_clicked(){
    age = "Over 65";
}


// for Radio Buttom, choose shot, first or second shot
void UserDialog1::on_radioButton_firstShot_clicked(){
    shot = "First shot";
}

void UserDialog1::on_radioButton_secondShot_clicked(){
    shot = "Seecond shot";
}

void UserDialog1::on_comboBox_hours_activated(const QString &arg1){
    tim = arg1;
}

```
</h4></p>
  
<p align=left>  <h1 align="left">D. UserDialog1.h:</12></p>
  
<p align=left>  <h4 align="left">  
  
  ```cpp
  #ifndef USERDIALOG1_H
#define USERDIALOG1_H

#include <QDialog>
#include "UserDL2.h"

namespace Ui {
class UserDialog1;
}

class UserDialog1 : public QDialog
{
    Q_OBJECT

public:

    explicit UserDialog1(QWidget *parent = nullptr);
    ~UserDialog1();

    QString user_name;    // make suer the longin user and edit their own information. Make is same person.
    QString pw;

private slots:

    void on_pushButton_userNextPage_clicked();

    void on_radioButton_Jonson_clicked();

    void on_radioButton_moderna_clicked();

    void on_radioButton_pfizer_clicked();


    void on_radioButton_under18_clicked();

    void on_radioButton_over18_clicked();

    void on_radioButton_over50_clicked();

    void on_radioButton_over65_clicked();


    void on_radioButton_firstShot_clicked();

    void on_radioButton_secondShot_clicked();


    void on_comboBox_hours_activated(const QString &arg1);

private:
    Ui::UserDialog1 *ui;
    UserDL2 *userP2;

};

#endif // USERDIALOG1_H
  
  ```
  
  </h4></p>
  
 <p align=left>  <h1 align="left"> E. AdminDialog1.cpp:</12></p>
  
<p align=left>  <h4 align="left">
  
```cpp
#include "AdminDialog1.h"
#include "ui_AdminDialog1.h"
#include "Login.h"
#include <QString>
#include <QMessageBox>
#include <QPixmap>
#include <QTableView>
#include <QListView>
#include <QLineEdit>
#include <QSqlTableModel>
#include <QComboBox>
#include "user.h"

AdminDialog1::AdminDialog1(QWidget *parent) :
    QDialog(parent),
    ui(new Ui::AdminDialog1){
    ui->setupUi(this);
}

AdminDialog1::~AdminDialog1(){
    delete ui;
}

void AdminDialog1::on_pushButton_loadInfo_clicked(){
    Login conn;
    QSqlQueryModel * modal = new QSqlQueryModel();  // create model
    conn.connOpen();
    QSqlQuery* qry = new QSqlQuery(conn.mydb);

    qry->prepare("select username,password,insurance,firstName,lastName,age,vaccine,shot,date,time from userInfo");
    qry->exec();
    modal->setQuery(*qry);

    ui->listView->setModel(modal);
    ui->comboBox->setModel(modal);

    ui->tableView->setModel(modal);

    conn.connClose();
    qDebug()<<(modal->rowCount());

}

void AdminDialog1::on_comboBox_currentIndexChanged(const QString &arg1){
    QString username = ui->comboBox->currentText();
    Login conn;
    if(!conn.connOpen()){
        qDebug() <<"Failed to open the database";
        return;
    }
    conn.connOpen();
    QSqlQuery qry;

    qry.prepare("select * from userInfo where username='"+username+"'");
    qDebug()<<qry.executedQuery();

    if(qry.exec()) {
        while(qry.next()){
            ui->lineEdit_username->setText(qry.value(0).toString());
            ui->lineEdit_pw->setText(qry.value(1).toString());
            ui->lineEdit_insur->setText(qry.value(2).toString());
            ui->lineEdit_fName->setText(qry.value(3).toString());
            ui->lineEdit_lName->setText(qry.value(4).toString());
            ui->lineEdit_age->setText(qry.value(5).toString());
            ui->lineEdit_vaccine->setText(qry.value(6).toString());
            ui->lineEdit_shot->setText(qry.value(7).toString());
            ui->lineEdit_date->setText(qry.value(8).toString());
            ui->lineEdit_time->setText(qry.value(9).toString());
        }
        conn.connClose();
    }else{
        QMessageBox::critical(this,tr("Error"),qry.lastError().text());
    }
}


void AdminDialog1::on_tableView_activated(const QModelIndex &index){

    QString val = ui->tableView->model()->data(index).toString();
    Login conn;
    if(!conn.connOpen()){
        qDebug() <<"Failed to open the database";
        return;
    }
    conn.connOpen();
    QSqlQuery qry;
    qry.prepare("select * from userInfo where username='"+val+"' or password='"+val+"' or insurance='"+val+"' or firstName='"+val+"' or lastName='"+val+"'"
             " or age='"+val+"' or vaccine='"+val+"' or shot='"+val+"' or date='"+val+"' or time='"+val+"' ");

    qDebug()<<qry.executedQuery();
    if(qry.exec()) {

        while(qry.next()){
            ui->lineEdit_username->setText(qry.value(0).toString());
            ui->lineEdit_pw->setText(qry.value(1).toString());
            ui->lineEdit_insur->setText(qry.value(2).toString());
            ui->lineEdit_fName->setText(qry.value(3).toString());
            ui->lineEdit_lName->setText(qry.value(4).toString());
            ui->lineEdit_age->setText(qry.value(5).toString());
            ui->lineEdit_vaccine->setText(qry.value(6).toString());
            ui->lineEdit_shot->setText(qry.value(7).toString());
            ui->lineEdit_date->setText(qry.value(8).toString());
            ui->lineEdit_time->setText(qry.value(9).toString());
        }
        conn.connClose();
    }else{
        QMessageBox::critical(this,tr("Error"),qry.lastError().text());
    }
}


void AdminDialog1::on_listView_activated(const QModelIndex &index){

    QString val = ui->listView->model()->data(index).toString();
    Login conn;

    if(!conn.connOpen()){
        qDebug() <<"Failed to open the database";
        return;
    }
    conn.connOpen();
    QSqlQuery qry;

    qry.prepare("select * from userInfo where username='"+val+"' ");

    qDebug()<<qry.executedQuery();
    if(qry.exec()) {
        while(qry.next()){
            ui->lineEdit_username->setText(qry.value(0).toString());
            ui->lineEdit_pw->setText(qry.value(1).toString());
            ui->lineEdit_insur->setText(qry.value(2).toString());
            ui->lineEdit_fName->setText(qry.value(3).toString());
            ui->lineEdit_lName->setText(qry.value(4).toString());
            ui->lineEdit_age->setText(qry.value(5).toString());
            ui->lineEdit_vaccine->setText(qry.value(6).toString());
            ui->lineEdit_shot->setText(qry.value(7).toString());
            ui->lineEdit_date->setText(qry.value(8).toString());
            ui->lineEdit_time->setText(qry.value(9).toString());
        }
        conn.connClose();
    }else{
        QMessageBox::critical(this,tr("Error"),qry.lastError().text());
    }
}

void AdminDialog1::on_pushButton_save_clicked(){  // insert
    Login conn;
    User user;
    user.username = ui->lineEdit_username->text();
    user.password = ui->lineEdit_pw->text();
    user.insurance = ui->lineEdit_insur->text();
    user.firstName = ui->lineEdit_fName->text();
    user.lastName = ui->lineEdit_lName->text();
    user.age = ui->lineEdit_age->text();
    user.vaccine = ui->lineEdit_vaccine->text();
    user.shot = ui->lineEdit_shot->text();
    user.date = ui->lineEdit_date->text();
    user.time = ui->lineEdit_time->text();
    if(!conn.connOpen()){
        qDebug() <<"Failed to open the database";
        return;
    }
    conn.connOpen();
    QSqlQuery qry;
    qry.prepare("insert into userInfo (username,password,insurance,firstName,lastName,age,vaccine,shot,date,time) "
     "values ('"+user.username+"','"+user.password+"','"+user.insurance+"','"+user.firstName+"','"+user.lastName+"','"+user.age+"','"+user.vaccine+"','"+user.shot+"','"+user.date+"','"+user.time+"' )" );

    qDebug()<<qry.executedQuery();

    if(qry.exec()) {
        QMessageBox::information(this,tr("Save"),tr("Data Saved!"));
        conn.connClose();
    }else{
        QMessageBox::critical(this,tr("Error"),qry.lastError().text());
    }
    qDebug()<< user.username + " saved her (his) infomation: ";
    qDebug()<< user.username + " , " + user.password + " , " + user.insurance + " , " + user.firstName + " , " + user.lastName +
               " , " + user.age + " , " + user.vaccine + " , " + user.shot + " , " + user.date + " , " + user.time;
}


void AdminDialog1::on_pushButton_update_clicked(){  // update
    Login conn;
    User user;
    user.username = ui->lineEdit_username->text();
    user.password = ui->lineEdit_pw->text();
    user.insurance = ui->lineEdit_insur->text();
    user.firstName = ui->lineEdit_fName->text();
    user.lastName = ui->lineEdit_lName->text();
    user.age = ui->lineEdit_age->text();
    user.vaccine = ui->lineEdit_vaccine->text();
    user.shot = ui->lineEdit_shot->text();
    user.date = ui->lineEdit_date->text();
    user.time = ui->lineEdit_time->text();

    if(!conn.connOpen()){
        qDebug() <<"Failed to open the database";
        return;
    }
    conn.connOpen();
    QSqlQuery qry;

    qry.prepare("update userInfo set username='"+user.username+"',password='"+user.password+"',insurance='"+user.insurance+"',firstName='"+user.firstName+"',lastName='"+user.lastName+"'"
             ",age='"+user.age+"',vaccine='"+user.vaccine+"',shot='"+user.shot+"',date='"+user.date+"',time='"+user.time+"' where username='"+user.username+"'" );

    qDebug()<<qry.executedQuery();

    if(qry.exec()) {
        QMessageBox::information(this,tr("Edit"),tr("Data Updated!"));
        conn.connClose();
    }else{
        QMessageBox::critical(this,tr("Error"),qry.lastError().text());
    }

    qDebug()<< user.username + " Updated infomation: ";
    qDebug()<< user.username + " , " + user.password + " , " + user.insurance + " , " + user.firstName + " , " + user.lastName +
               " , " + user.age + " , " + user.vaccine + " , " + user.shot + " , " + user.date + " , " + user.time;

}


void AdminDialog1::on_pushButton_delete_clicked(){ // delete
    Login conn;
    User user;
    user.username = ui->lineEdit_username->text();   //only need a unique variable, find whole row and delete

    if(!conn.connOpen()){
        qDebug() <<"Failed to open the database";
        return;
    }
    conn.connOpen();
    QSqlQuery qry;

    qry.prepare("Delete from userInfo where username='"+user.username+"'");
    qDebug()<<qry.executedQuery();

    if(qry.exec()) {
        QMessageBox::information(this,tr("Delete"),tr("Data Deleted!"));
        conn.connClose();
    }else{
        QMessageBox::critical(this,tr("Error"),qry.lastError().text());
    }

    qDebug()<< user.username + " deleted infomation: ";
    qDebug()<< user.username + " , " + user.password + " , " + user.insurance + " , " + user.firstName + " , " + user.lastName +
               " , " + user.age + " , " + user.vaccine + " , " + user.shot + " , " + user.date + " , " + user.time;
}

void AdminDialog1::on_pushButton_quit_clicked(){
    QMessageBox::StandardButton reply = QMessageBox::question(this,"My Title","Are you sure to quit the login page? ", QMessageBox::Yes| QMessageBox::No);
    if(reply == QMessageBox::Yes){
        QApplication::quit();
    }else{
        qDebug() << "Answer 'NO' Button is clicked";
    }
}

  ```
 </12></p> 
  
 </p>
 
 <p>
  <p align=left>  <h1 align="left"> E. AdminDialog1.h:</12></p>
  
  <p align=left>  <h4 align="left">
  
  ```cpp
 #ifndef ADMINDIALOG1_H
#define ADMINDIALOG1_H

#include <QDialog>


namespace Ui {
class AdminDialog1;
}

class AdminDialog1 : public QDialog
{
    Q_OBJECT

public:

    explicit AdminDialog1(QWidget *parent = nullptr);
    ~AdminDialog1();

private slots:
    void on_pushButton_loadInfo_clicked();

    void on_comboBox_currentIndexChanged(const QString &arg1);

    void on_tableView_activated(const QModelIndex &index);

    void on_listView_activated(const QModelIndex &index);

    void on_pushButton_update_clicked();

    void on_pushButton_save_clicked();

    void on_pushButton_delete_clicked();

    void on_pushButton_quit_clicked();

private:
    Ui::AdminDialog1 *ui;

};

#endif // ADMINDIALOG1_H
  
  ```
  </h4></p>
  
 </p>
 
 * * *
 
 <p>
  <p align=left>  <h1 align="left"> 5. Pour la page utilisateur : (Button Register):</h1></p>
 </p>
 
 Pour les personnes qui utilisent ce système et qui se connectent pour la première fois. Vous devez d’abord entrer deux choses, et faire 
ils sont tous corrects, et se souviennent aussi d’eux. Pour le nom d’utilisateur et le mot de passe, il peut s’agir d’une chaîne, d’un caractère, 
nombre. Le nom d’utilisateur est Unique.


Une fois que l’utilisateur a entré deux informations, il enregistrera le nom d’utilisateur et le mot de passe dans la base de données. 
de retour dans le système. Donc, la prochaine fois, cet administrateur a ses informations enregistrées dans la base de données.

 ## Manuels d’utilisation et de logiciels
 
![registrer](https://user-images.githubusercontent.com/93833171/151636964-d7670f3d-b287-4e0d-9bf5-41ba66008724.png)

 <p>
  <p align=left>  <h1 align="left"> 6. Pour la page utilisateur : (Button Login) et boîte de dialogue suivante</h1></p>
 </p>
 Pour les personnes qui utilisent avec ce système, vous devez d’abord entrer deux choses et vous assurer que 
le nom d’utilisateur et le mot de passe sont corrects en même temps. 

Ensuite, après avoir cliqué sur le bouton de connexion, les informations correspondront à la base de données. 
Sinon, il affichera une erreur jusqu’à ce que vous saisissiez le nom d’utilisateur et le mot de passe corrects.

#### cas 1: on entre le nom d'utilisateur n'est pas correct:
![login](https://user-images.githubusercontent.com/93833171/151637727-a5f11d58-9f52-4467-a1c7-9aec2e766bd2.png)

#### cas 2:on assure le nom d’utilisateur et le mot de passe sont corrects en même temps.
On Pasee directement à d'autres interface:

![interface](https://user-images.githubusercontent.com/93833171/151639419-8440f1cd-d7a6-4994-9f8a-295b5ad28b49.jpg)
Vous pouvez commencer à construire et enregistrer votre imformation comme ceci:


![belcaid_inscrire](https://user-images.githubusercontent.com/93833171/151643277-17bdd714-1c28-499d-b76a-710887c4313c.jpg)

Après avoir cliqué sur le Botton suivant, toutes vos informations sont enregistrées dans le système de base de données.

![said_belcaid](https://user-images.githubusercontent.com/93833171/151643844-d38ced56-f2d1-45f3-aaaa-e55cdc8f8256.png)


 <p>
  <p align=left>  <h1 align="left"> 7.Pour la page d'administration : (Button Register)</h1></p>
 </p>
Pour les personnes qui travaillent en tant qu’administrateur avec ce système, et la première connexion. Vous devez entrer 
trois choses d’abord, et faites-les toutes correctes, et souvenez-vous aussi d’elles. Pour le nom d’utilisateur 
et Mot de passe, il peut s’agir d’une chaîne, d’un caractère, d’un nombre. Mais pour le code de travail, il doit s’agir d’un 
nombre. Le nom d’utilisateur est Unique.
Après avoir entré l’administrateur pour ces trois informations, il enregistrera le nom d’utilisateur, le mot de passe et 
code de travail dans la base de données dans le système. Donc la prochaine fois, cet admin a son/elle 
les informations enregistrées dans la base de données. Cela ressemblera à ceci:

![zineb1](https://user-images.githubusercontent.com/93833171/151642158-5afc5da9-1925-4a5c-b73f-6d1698dced82.png)

 <p>
  <p align=left>  <h1 align="left"> 8.Pour la page d'administration : (Button login)</h1></p>
 </p>

Si vous avez fait les choses correctes comme vous vous êtes inscrit à la première fois. La deuxième boîte de dialogue sautera 
comme ça.

![belcaid](https://user-images.githubusercontent.com/93833171/151642811-b5897141-4c06-48c6-8d5f-0a6d7f088217.jpg)

Ensuite, après avoir cliqué sur le bouton « **Load Data** », toutes les données de la base de données seront 
télécharger sur la vue de tableau( la zone noire et aussi chaque textEditLine.
En outre, il y a trois boutons qui sont cliquables.

**Save button:** Pour enregistrer toute l’imformation du côté de l’administrateur. Entrez toute l’imformation 
sur le texte a fait une ligne, puis appuyez sur le bouton Enregistrer. Le rendez-vous vaccinal de l’utilisateur sera 
sauvé.

**Update button:** Pour modifier et mettre à jour l’imformation à partir du système de base de données, au cas où, 
n’importe qui veut changer l’imformation qu’il a enregistrée auparavant.

**Delete button:** pour supprimer toute imformation que l’utilisateur a enregistrée auparavant.
Votre écran ressemblera à ceci:

![s_z_a](https://user-images.githubusercontent.com/93833171/151660913-c6ac0832-13c7-4002-8e8a-48e51cb1b4a4.jpg)

une fois on clique sur le bouton **saved**.les information sont enregister a cote de l'administrateur:
![saved](https://user-images.githubusercontent.com/93833171/151661410-3a339813-d9b6-47bd-835c-c9c94bd7ebe9.jpg)

 <p>
  <p align=left>  <h1 align="left"> 9.Pour la page principale (bouton quitter)</h1></p>
 </p>
Une fois que l’utilisateur a cliqué sur le bouton Quitter, la boîte Qmessage saute:Oui pour **quitter** le système, non pour **rester**.


![quiter](https://user-images.githubusercontent.com/93833171/151661855-a377ed3c-ccb3-439d-9406-81cac0e6bf22.jpg)

* * *
![titre](https://user-images.githubusercontent.com/93833171/151644405-8e4e79c2-22b6-4cf1-a974-0bf2bf4eb3d2.jpg)
