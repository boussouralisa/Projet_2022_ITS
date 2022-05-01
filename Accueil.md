# Projet_2022_ITS
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
print(__name__)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    viewA = View_A()
    sys.exit(app.exec_())
