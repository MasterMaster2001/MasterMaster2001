
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


![logo](https://user-images.githubusercontent.com/93833171/151628427-cfe54515-74d0-4b50-980c-e016c791722e.jpg)
* * *
<p>
  <p align=left>  <h1 align="left">4. bibliothèques nécessaires à la compilation de ce programme (pour chaque fichier):</12></p>
 </p>
 
 * * *
 <p>
  <p align=left>  <h1 align="left">A. main.cpp :</h1></p>
  
  ```cpp
#include <QApplication>
#include <QTabWidget> 
#include <QPushButton

  ```
   <p align=left>  <h1 align="left">B. Login.cpp:</h1></p>
   
  ```cpp
#include <QSqlError>
#include <QSqlQuery>
#include <QDebug>
#include <QString>
#include <QException>
#include <QMessageBox>

   ```
<p align=left>  <h1 align="left">C. User.cpp:</h1></p>
    
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
 
 

