
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

![titre](https://user-images.githubusercontent.com/93833171/151644405-8e4e79c2-22b6-4cf1-a974-0bf2bf4eb3d2.jpg)





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
 10. UML, test unitaire pour chaque classe (maintenance du système)
 11. Contactez le fabricant


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
  <p align=left>  <h1 align="left">A. main.cpp :</12></p>
  
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
   <p align=left>  <h1 align="left">B. Login.cpp:</12></p>
   
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
<p align=left>  <h1 align="left">B. Login.h:</12></p>
     
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
<p align=left>  <h1 align="left">C. User.cpp:</12></p>
    
 ```cpp
#include <iostream>
#include <QString>

 ```
<p align=left>  <h1 align="left">D. UserDialog1.cpp:</12></p>
  
  ```cpp
#include <QPixmap>
#include <QMessageBox>
#include <QDebug>

  ```
 <p align=left>  <h1 align="left"> E. AdminDialog1.cpp:</12></p>
  
```cpp
#include <QString>
#include <QMessageBox>
#include <QPixmap>
#include <QTableView>
#include <QListView>
#include <QLineEdit>
#include <QSqlTableModel>
#include <QComboBox>
  ```
     
 </p>
 
    <p align=left>  <h1 align="left">B. Login.cpp:</12></p>
 
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

#### cas 1:on assure le nom d’utilisateur et le mot de passe sont corrects en même temps.
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

Si vous avez fait les choses correctes comme vous vous êtes inscrit à la première fois. La deuxième boîte de dialogue sautera 
comme ça.

![belcaid](https://user-images.githubusercontent.com/93833171/151642811-b5897141-4c06-48c6-8d5f-0a6d7f088217.jpg)

Ensuite, après avoir cliqué sur le bouton « **Load Data** », toutes les données de la base de données seront 
télécharger sur la vue de tableau( la zone noire et aussi chaque textEditLine.
En outre, il y a trois boutons qui sont cliquables.
**Bouton Enregistrer :** Pour enregistrer toute l’imformation du côté de l’administrateur. Entrez toute l’imformation 
sur le texte a fait une ligne, puis appuyez sur le bouton Enregistrer. Le rendez-vous vaccinal de l’utilisateur sera 
sauvé.

**Bouton Mettre à jour:** Pour modifier et mettre à jour l’imformation à partir du système de base de données, au cas où, 
n’importe qui veut changer l’imformation qu’il a enregistrée auparavant.

**Bouton Supprimer :** pour supprimer toute imformation que l’utilisateur a enregistrée auparavant.
