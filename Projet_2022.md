# Projet_2022_ITS

import dbm
import sys
import os
from PyQt5 import QtCore
from PyQt5.QtGui import (QIntValidator, QPixmap, QIcon,QFont)
from PyQt5.QtWidgets import (QTextEdit,QLineEdit, QPushButton, QVBoxLayout, QHBoxLayout,
                             QApplication, QMainWindow, QWidget, QLabel, QToolTip, 
                             QToolButton, QGridLayout, QHBoxLayout, QRadioButton)
from PyQt5 import QtCore, QtWidgets

viewA=None
dbmfile = dbm.open('Suivi Patient', 'c')
dbmfile['']='Patient'
#%%       
class View_C(QtWidgets.QWidget):
 
    # crée un nouveau signal qui permettra, si nécessaire, de renvoyer des 
    # données à l'appelant
    fermeturefen2 = QtCore.pyqtSignal()
 
   
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Créer un dossier")
        self.resize(500, 400)
        self.Patient = QLineEdit('')
        
        #répertoire pour les médicaments
        self.repertoire={"Doliprane":['courbature','fievre','paracetamol','gelules','maux de tete','douleurs'],
                         "Dafalgan":['courbature','fievre','paracetamol','effervescent','maux de tete','douleurs','douleurs dentaires'],
                         "Efferalgant":['courbatture','fievre','paracetamol','effervescent','maux de tete'],
                         "Kardegic":['vasculaires','cérébral','cardiaque','infractus'],
                         "Spasfon":['douleur intestin',"digestif","contraction", 'effervescent'],
                         "Gaviscon":['brulures','estomac','indigestion'],
                         "Dexeryl":['irritation','cutanées', 'brulures'],
                         "Meteospasmyl":['digestion','ballonement','tube digestif'],
                         "Biseptine":["lésion","cutanées","infection",'plaies'],
                         "Eludril":['infecion','gencives', 'dents','saignement bouche']}
        
        self.init_ui()
        
    def init_ui(self):
        #horizontale box
        vbox = QVBoxLayout()
        #verticale box
        hbox = QHBoxLayout()
        
        grid = QGridLayout()
        
        #un formulaire avec une étiquette, un champ de saisie
        self.Nom = QLabel('Nom : ')
        self.NomEdit = QLineEdit()
        grid.addWidget(self.Nom, 0, 0)
        grid.addWidget(self.NomEdit, 0, 1)
        
        
        Prenom = QLabel('Prenom : ')
        PrenomEdit = QLineEdit()
        grid.addWidget(Prenom, 1, 0)
        grid.addWidget(PrenomEdit, 1, 1)
        
        Age = QLabel('Age : ')
        AgeEdit = QLineEdit()
        grid.addWidget(Age, 2, 0)
        grid.addWidget(AgeEdit, 2, 1)
        
        Sexe = QLabel('Sexe:')
        self.SexeM = QRadioButton('Masculin')
        self.SexeF = QRadioButton('Féminin')
        grid.addWidget(Sexe, 3, 0)
        grid.addWidget(self.SexeM, 3, 1)
        grid.addWidget(self.SexeF, 3, 2)
        layout = QVBoxLayout()
        layout.addWidget(self.SexeM)
        layout.addWidget(self.SexeF)
        
        Symptome = QLabel('Symptome:')
        SymptomeTextEdit = QTextEdit()
        grid.addWidget(Symptome, 4, 0)
        grid.addWidget(SymptomeTextEdit, 4, 1)
        self.s_Symptome=QTextEdit()
        self.s_Symptome.textChanged.connect(self.Medicament)
        
        
        Ordonnance = QLabel('Ordonnance :')
        OrdonnanceTextEdit = QTextEdit()
        grid.addWidget(Ordonnance, 5, 0)
        grid.addWidget(OrdonnanceTextEdit, 5, 1)
 
        self.Affichage= QLabel('Affichage:')
        self.AffichageTextEdit = QTextEdit()
        grid.addWidget(self.Affichage, 6, 0)
        grid.addWidget(self.AffichageTextEdit, 6, 1)
        
        self.Voir = QLabel(" ")
        self.VoirTextEdit=QTextEdit()
        grid.addWidget(self.Voir, 7, 0)
        grid.addWidget(self.Voir, 7, 1)
        self.VoirTextEdit.textChanged.connect(self.Medicament)
        vbox.addLayout(grid)#champ de formulaire
        
        
        self.enregistrerbutton = QToolButton()
        self.enregistrerbutton.setText('Enregistrer')
        
        self.historiquebutton = QToolButton()
        self.historiquebutton.setText('Historique')
        
        self.fermebutton = QToolButton()
        self.fermebutton.setText('Cancel')
        
        #pour fermer la page quand on clique Cancel
        self.fermebutton.clicked.connect(self.Cancel)
        
        
        hbox.addWidget(self.historiquebutton)
        self.historiquebutton=QPushButton('Historique', clicked = lambda : self.historique(self.ViewA))
        hbox.addWidget(self.enregistrerbutton)
        hbox.addWidget(self.fermebutton)
        vbox.addLayout(hbox)
        self.setLayout(vbox)

    def enregistrement(self, Patient):
        
      dbmfile = dbm.open('Suivi du Patient', 'c')
        #ajoute nom dans BD
      longueur = str(len(dbmfile.keys()) + 1 )
      print("longueur=",longueur)
      dbmfile[longueur] = Patient
        #ferme ma BD
      dbmfile.close()
        
    def Medicament(self):
        self.AffichageTextEdit.clear()
        self.Text=self.VoirTextEdit.toPlainText()
        self.Text=self.Text.split('\n')
        self.mot=[]
        for i in self.Text:
            self.ligne=i.split(' ')
            for j in self.ligne:
                self.mot.append(j)
        self.medicament=[]
        for k in range(len(self.mot)):
            for l in self.repertoire.keys():
                for m in self.repertoire[l]:
                    if self.mot[k]==m:
                        self.medicament.append(l)
                                  
        self.medicament=list(set(self.medicament))
        self.resultat=""
        for k in self.medicament:
            self.resultat+=k+'\n'
        self.AffichageTextEdit.setText(self.resultat)
        self.show()
        
    def historique(self,viewA):
        Possible=self.enregistrement()
        
        self.fichier=self.NomEdit+'_'+self.PrenomEdit+'_'+self.AgeEdit+'_'+self.Sexe+'_.txt' 
        user=dbmfile
        Liste=dbmfile
        
        Liste=user.SendCommand(self.fichier)
        
    #on ferme la fenetre
    def Cancel(self):
        
        self.close()
   
    def closeEvent(self, event):
        """à la fermeture de cette fenêtre 2, celle-ci envoie un signal à la 
           fenêtre 1 appelante
        """
        self.fermeturefen2.emit() 
        event.accept()

#%%

class View_I(QtWidgets.QWidget):
 
    # crée un nouveau signal qui permettra de renvoyer des données à l'appelant
    fermeturefen3 = QtCore.pyqtSignal()
    
    
    
    def __init__(self):
        super().__init__()
        
        #horizontale box
        vbox = QVBoxLayout()
        #verticale box
        hbox = QHBoxLayout()
        self.setWindowTitle("Importer un dossier")
        self.resize(500, 400)
        
        self.Voir = QToolButton()
        self.Voir.setText('Voir')
        
        
        self.ferme = QToolButton()
        self.ferme.setText('Cancel')
        
        self.recuperer = QToolButton()
        self.recuperer.setText('recuperer')
        
        #pour fermer la page quand on clique Cancel
        self.ferme.clicked.connect(self.Cancel)
        
        hbox.addWidget(self.Voir)
        hbox.addWidget(self.ferme)
        hbox.addWidget(self.recuperer)
        vbox.addLayout(hbox)
        self.setLayout(vbox)
        self.setWindowTitle('Importation')
        
    def recuperer(self):
        
        self.afficher=self.liste.currentText
        
        self.afficher=self.Fichier.split(' ')
        self.Fichier.pop()
        self.NomEdit=self.Fichier[0]
        self.PrenomEdit=self.Fichier[1]
        self.AgeEdit=self.Fichier[2]
        self.Sexe=self.Fichier[3]
        print(self.Sexe)
        
        self.view_C= View_C(View_A,self.NomEdit,self.PrenomEdit,self.AgeEdit,self.Sexe)
    
    def Voir(self):
        self.liste.clear()
        user=dbmfile
        Liste=user.sendCommand()
        
        for i in Liste:
            self.liste(i)
        #on ferme la fenetre
        
    def Cancel(self):
            
            self.close()
            

    def closeEvent(self, event):
    #à la fermeture de cette fenêtre 3, celle-ci envoie un signal à la  fenêtre 1 appelante
        
         self.fermeturefen3.emit() 
         event.accept()
#%%
class View_A(QtWidgets.QWidget):
    def __init__(self):
        super().__init__()
        
        
        self.setWindowTitle("Fenetre 1")
        self.resize(300, 200)
 
        # crée un bouton1
        self.bouton1 = QtWidgets.QPushButton("Créer un dossier")
        self.bouton1.setStyleSheet ("background-color : lightblue")
        self.bouton1.clicked.connect(self.appelfen2)
        
        #crée un bouton2
        self.bouton2 = QtWidgets.QPushButton("Importer un dossier")
        self.bouton2.setStyleSheet ("background-color : lightblue")
        self.bouton2.clicked.connect(self.appelfen3)
        
        #afficher le bienvenue et le suivi du patient
        self.e = QLabel("Suivi du patient \n \n  Bienvenue à la plateforme")
        
        #importation de l'image 
        self.pixmap = QPixmap('image.png')
        self.label = QLabel(self)
        self.label.setPixmap(self.pixmap)
        
        #dimensionner l'image
        self.label.setGeometry(520, 420, 400, 120)
        self.label.move(145,40)
        self.init_ui()
        
        #Afficher l'interface graphique
        self.show()
        
    def init_ui(self):
        h_box = QHBoxLayout()
        
        v_box = QVBoxLayout()
        self.setLayout(v_box)
        v_box.addWidget(self.e)
        v_box.addWidget(self.bouton1)
        v_box.addWidget(self.bouton2)
        h_box.addStretch()
        
        
        self.e.setAlignment(QtCore.Qt.AlignCenter)
        
        
        v_box.addLayout(h_box)

        self.setWindowTitle("Suivi du patient")
        self.setGeometry(80, 120, 320, 380)
 
        # initialise la variable de la fenêtre 2
        self.fenetre2 = None
        self.fenetre3 = None

    def appelfen2(self):
        """méthode appelée par le bouton, Lance la deuxième fenêtre
        """
        if self.fenetre2==None:
            self.fenetre2 = View_C()
            # prépare la future fermeture de la fenêtre 2 
            self.fenetre2.fermeturefen2.connect(self.fen2close)
            # affiche la 2ème fenêtre
            self.fenetre2.show()
 
    
    def fen2close(self):
        """méthode appelée par la fermeture de la fenêtre 2
        """
        self.fenetre2 = None
 
        
    def appelfen3(self):
        """méthode appelée par le bouton, Lance la deuxième fenêtre
        """
        if self.fenetre3==None:
            self.fenetre3 = View_I()
            # prépare la future fermeture de la fenêtre 2 
            self.fenetre3.fermeturefen3.connect(self.fen3close)
            # affiche la 2ème fenêtre
            self.fenetre3.show()
 
    
    def fen3close(self):
        """méthode appelée par la fermeture de la fenêtre 2
        """
        self.fenetre3 = None
    

    def closeEvent(self, event):
        """méthode appelée lors de la fermeture de la fenêtre 1
        """
        if self.fenetre2 != None:
        
            # la fenêtre 2 est encore ouverte: on la ferme!
            self.fenetre2.close()
            self.fenetre2 = None4
            print("Fenêtre 2 fermée")
            print("Fenêtre 1 fermée")
            event.accept() 
            
        if self.fenetre3 != None:
            # la fenêtre 2 est encore ouverte: on la ferme!
            self.fenetre3.close()
            self.fenetre3 = None
            print("Fenêtre 3 fermée")
            print("Fenêtre 1 fermée")
        event.accept() 
#%%

class Model:
    
    def __init__(self, ssh):
        #ouvre ma BD
        
        dbmfile = dbm.open('Suivi du Patient', 'c')
        Patient = len(dbmfile.keys())
        #ferme ma BD
        dbmfile.close()
        
    def View_C(self,Patient):
        
        dbmfile = dbm.open('Suivi du Patient', 'c')
        longueur = str(len(dbmfile.keys()) + 1 )
        print("longueur=",longueur)
        dbmfile[longueur] = Patient
        #ferme ma BD
        dbmfile.close()
        
    def View_A(self,Patient):
        dbmfile = dbm.open('Suivi du Patient', 'c')
        longueur = str(len(dbmfile.keys()) + 2 )
        print("longueur=",longueur)
        dbmfile[longueur] = Patient
        #ferme ma BD
        dbmfile.close()
        
    def View_I(self,Patient):
        
        dbmfile = dbm.open('Suivi du Patient', 'c')
        longueur = str(len(dbmfile.keys()) + 1 )
        print("longueur=",longueur)
        dbmfile[longueur] = Patient
        #ferme ma BD
        dbmfile.close()
    
  
#%% 
print(__name__)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    viewA = View_A()
    viewC = View_C()
    viewI = View_I()
    sys.exit(app.exec_())
       
        

