# Fourmi-de-Langton

    from tkinter import *
    from random import *
    from time import *

    T_CASE=15
    NB_LIG=60
    NB_COL=80
    COULEUR={'fourmi':'light green','plein':'black','vide':'white','contour':'black','fond':'pink'}
    MARGE=5
    VITESSE=0


    fen=Tk()
    grille=[]
    fourmi=[[randint(0,NB_LIG-1),randint(0,NB_COL-1),None,-1,0,VITESSE]]



    def quitter():
        fen.quit()
        fen.destroy()

    def coords(lig,col):
        return col*T_CASE+MARGE,lig*T_CASE+MARGE,(col+1)*T_CASE+MARGE,(lig+1)*T_CASE+MARGE


    def initGrille():
        for lig in range(NB_LIG):
            temp=[]
            for col in range(NB_COL):
                temp.append(canvas.create_rectangle(*coords(lig,col),fill=COULEUR['vide'],outline=COULEUR['contour']))
            grille.append(temp)


    def initFourmi():
        lig,col=fourmi[:2]
        fourmi[2]=canvas.create_rectangle(*coords(lig,col),fill=COULEUR['fourmi1'],outline=COULEUR['fourmi1'])


    def tournerG():
        dirLig,dirCol=fourmi[3],fourmi[4]
        if dirLig!=0:fourmi[3]=0
        else:fourmi[3]=-dirCol
        if dirCol!=0:fourmi[4]=0
        else:fourmi[4]=dirLig


    def tournerD():
        dirLig,dirCol=fourmi[3],fourmi[4]
        if dirLig!=0:fourmi[3]=0
        else:fourmi[3]=dirCol
        if dirCol!=0:fourmi[4]=0
        else:fourmi[4]=-dirLig


    def getCoul(lig,col):
        return canvas.itemcget(grille[lig][col],'fill')

    def setCoul(lig,col,coul):
        canvas.itemconfigure(grille[lig][col],fill=coul)

    def swapCoul(lig,col):
        if getCoul(lig,col)==COULEUR['vide']:
            setCoul(lig,col,COULEUR['plein'])
        else:
            setCoul(lig,col,COULEUR['vide'])

    def avance():
        (lig,col)=fourmi[:2]
        if getCoul(lig,col)==COULEUR['vide']:tournerD()
        else:tournerG()
        swapCoul(lig,col)
        fourmi[0]=(fourmi[0]+fourmi[3])%NB_LIG
        fourmi[1]=(fourmi[1]+fourmi[4])%NB_COL
        canvas.coords(fourmi[2],*coords(fourmi[0]],fourmi[1]))


    def lancer():
        for i in range(10000):
            avance()
            sleep(VITESSE)
            fen.update()


#Frame


    fCanvas=Frame(fen,width=NB_COL*T_CASE+2*MARGE)
    fCanvas.pack(side=LEFT)

    fJeu=Frame(fen,width=200)
    fJeu.pack(side=RIGHT)


#Canvas


    canvas=Canvas(fCanvas,height=T_CASE*NB_LIG+2*MARGE,width=T_CASE*NB_COL+2*MARGE,bg=COULEUR['fond'])
    canvas.pack()


#Button


    bLancer=Button(fJeu,width=15,font=('helvetica',12),text='Lancer une fourmi',command=lancer)
    bLancer.grid(row=0,column=0,padx=5,pady=5)

    bQuitter=Button(fJeu,width=10,font=('helvetica',12), text="Quitter", command=quitter)
    bQuitter.grid(row=3,column=1,padx=5,pady=5)


    if __init__ =="__main__":
      initGrille()
      initFourmi()
      fen.mainloop()
