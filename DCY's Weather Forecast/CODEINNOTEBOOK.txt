import sys
from selenium import webdriver
from PyQt5.QtWidgets import QMainWindow,QApplication,QWidget,QLabel,QPushButton,QLineEdit,QMessageBox,QDialog,QTextEdit,QVBoxLayout
from PyQt5.QtGui import QFont,QPixmap,QIcon
from PyQt5.QtCore import pyqtSlot
import time




class Pencere(QWidget):
    def __init__(self):

        super().__init__()

        self.init_ui()
    def init_ui(self):
        self.setWindowTitle("DCY's Weather Forecast Program")
        self.setGeometry(720,100,550,600)
        self.setStyleSheet("background-color:black")
        self.setWindowIcon(QIcon("C:/Users/90542/Desktop/havadurumu.png"))
        label = QLabel(self)
        a = QPixmap("C:/Users/90542/Desktop/dnn.png")
        label.setPixmap(a)
        label.move(110,0)

        label2 = QLabel("Made By DCY",self)
        label2.setStyleSheet("color:white")
        label2.setFont(QFont("Arial",10))
        label2.move(450,570)

        label3 = QLabel("Weather Forecast",self)
        label3.setStyleSheet("color:blue")
        label3.move(150,260)
        label3.setFont(QFont("Arial",20))

        self.satir = QLineEdit(self)
        self.satir.setStyleSheet("Background-color:white")
        self.satir.resize(350,50)
        self.satir.move(40,300)

        self.button = QPushButton("Search",self)
        self.button.setStyleSheet("Background-color:Grey")
        self.button.resize(100,50)
        self.button.move(400,300)
        self.button.setFont(QFont("Arial",20))

        self.button.clicked.connect(self.havadurumu)
        self.show()

    @pyqtSlot()
    def havadurumu(self):
        inputbarv= self.satir.text()

        url = "https://www.accuweather.com/en/search-locations?query="
        browser = webdriver.Firefox()
        browser.get(url + inputbarv)
        time.sleep(2)
        buttton = browser.find_element_by_xpath("/html/body/div/div[4]/div[1]/div[1]/div[2]/a[1]")
        buttton.click()
        time.sleep(10)
        celcius1 = browser.find_element_by_xpath(
            "/html/body/div/div[5]/div[1]/div[1]/a[1]/div[1]/div[1]/div/div/div[1]").text
        durum1 = browser.find_element_by_xpath(
            "/html/body/div/div[5]/div[1]/div[1]/a[1]/div[2]/span[1]").text
        durum2 = browser.find_element_by_xpath(
            "/html/body/div/div[5]/div[1]/div[1]/div[5]/a/div[2]/div[1]/div/div[1]").text
        durum3 = browser.find_element_by_xpath(
            "/html/body/div/div[5]/div[1]/div[1]/div[5]/a/div[2]/div[2]").text



        dlg = QDialog(self)
        dlg.setGeometry(100,100,500,600)
        self.setWindowIcon(QIcon("C:/Users/90542/Desktop/havadurumu.png"))

        labelimg = QLabel(dlg)
        self.pixmap = QPixmap("C:/Users/90542/Desktop/dnn.png")
        labelimg.setPixmap(self.pixmap)
        labelimg.move(100, 0)


        label3 = QLabel("Weather forecast", dlg)
        label3.setStyleSheet("color:blue")
        label3.move(150, 260)
        label3.setFont(QFont("Arial", 20))






        self.yazialani = QTextEdit(dlg)
        self.yazialani.move(0,300)
        self.yazialani.resize(500,300)
        self.yazialani.setStyleSheet("color:white")
        self.yazialani.setReadOnly(True)
        self.yazialani.setFont(QFont("Arial",10))

        self.yazialani.insertHtml(f' <p style="text-align:center;">Temperature:{celcius1} <br>')
        self.yazialani.insertHtml(f'Current Weather: {durum1} <br>')
        self.yazialani.insertHtml(f'Tomorrow Temperature: {durum2} <br>')
        self.yazialani.insertHtml(f'Tomorrow Weather Forecast: {durum3} <br>')




        dlg.exec()


































app = QApplication(sys.argv)
pencere = Pencere()
sys.exit(app.exec_())