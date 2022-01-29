
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


![noms](https://user-images.githubusercontent.com/93833171/151598206-f3763dd3-1a13-4fc5-983e-7aac49fe41c6.jpg)



* * *
## Introduction:
  1. Objectif
  2. La mise en œuvre nécessaire pour ce programme.
  3. Page de connexion principale (5 boutons peuvent effectuer différentes actions)
  4. bibliothèques nécessaires à la compilation de ce programme                                          (pour chaque fichier).
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


![nouvelle_interface](https://user-images.githubusercontent.com/93833171/151639613-aa6d3853-585a-4136-b248-1a72b68b7483.jpg)


* * *
<p>
  <p align=left>  <h1 align="left">4. bibliothèques nécessaires à la compilation de ce programme (pour chaque fichier):</12></p>
 </p>
 
 * * *
 <p>
  <p align=left>  <h1 align="left">A. main.cpp :</12></p>
  
  ```cpp
#include <QApplication>
#include <QTabWidget> 
#include <QPushButton

  ```
   <p align=left>  <h1 align="left">B. Login.cpp:</12></p>
   
  ```cpp
#include <QSqlError>
#include <QSqlQuery>
#include <QDebug>
#include <QString>
#include <QException>
#include <QMessageBox>

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
