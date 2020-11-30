# Databases
--CRÉATION DES TABLES
CREATE TABLE Clubs( 
Id NUMBER, 
ClubNom VARCHAR2(50 CHAR) NOT NULL,
Quartier VARCHAR2(50 CHAR) NOT NULL,
AnnéeCréation NUMBER NOT NULL,
Telephone VARCHAR2(20 CHAR) NOT NULL,
FraisInscription NUMBER(19,4) NOT NULL, 
CONSTRAINT clubs_pk PRIMARY KEY(Id)
);

CREATE TABLE Patrons(  
Id NUMBER, 
PrénomNom VARCHAR2(50 CHAR)NOT NULL,
Emploi VARCHAR2(20 CHAR)NOT NULL,
DateNaissance DATE NOT NULL,
Adresse VARCHAR2(50 CHAR)NOT NULL,
CONSTRAINT patrons_pk PRIMARY KEY(Id)
);
 
CREATE TABLE Chiens(  
Id NUMBER, 
IdClub NUMBER NOT NULL,
IdPatron NUMBER NOT NULL,
ChienNom VARCHAR2(20 CHAR)NOT NULL,
Race VARCHAR2(20 CHAR)NOT NULL,
AnnéeNaissance NUMBER NOT NULL,
Prix NUMBER(19,4) NOT NULL,
CONSTRAINT chiens_pk PRIMARY KEY(Id),
CONSTRAINT chiens_clubs_fk FOREIGN KEY (IdClub) REFERENCES Clubs (Id),  
CONSTRAINT chiens_patrons_fk FOREIGN KEY (IdPatron) REFERENCES Patrons (Id)   
);
 
CREATE TABLE Competitions(  
Id NUMBER, 
CompetitionNom VARCHAR2(50 CHAR)NOT NULL,
CompetitionDate DATE NOT NULL,
Frais NUMBER(19,4) NOT NULL,
NombreAudience NUMBER NOT NULL,
CONSTRAINT competitions_pk PRIMARY KEY(Id)
);

CREATE TABLE Prix(  
Id NUMBER, 
IdChien NUMBER NOT NULL,
IdCompetition NUMBER NOT NULL,
Prix VARCHAR2(20 CHAR)NOT NULL,
CONSTRAINT prix_pk PRIMARY KEY(Id),
CONSTRAINT prix_chiens_fk FOREIGN KEY(IdChien)REFERENCES Chiens(Id),
CONSTRAINT prix_competitions_fk FOREIGN KEY(IdCompetition) REFERENCES Competitions(Id)
);

CREATE TABLE ChiensCompetitions(  
IdCompetition NUMBER NOT NULL, 
IdChien NUMBER NOT NULL,
CONSTRAINT ChiensCompetitions_Competitions_fk FOREIGN  KEY(IdCompetition)REFERENCES Competitions(Id),
CONSTRAINT ChiensCompetitions_Chiens_fk FOREIGN KEY(IdChien) REFERENCES Chiens(Id)
);

--INSÉRTION DES DONNÉES

INSERT INTO Clubs (Id, ClubNom, Quartier, AnnéeCréation, Telephone, FraisInscription) VALUES (1, 'Club de Paris', 'Centre ville', 1995, '+176465451', 200);
INSERT INTO Clubs (Id, ClubNom, Quartier, AnnéeCréation, Telephone, FraisInscription) VALUES (2, 'Club de Lyon', 'Vieux-Lyon', 1990, '+145426862', 450);
INSERT INTO Clubs (Id, ClubNom, Quartier, AnnéeCréation, Telephone, FraisInscription) VALUES (3, 'Club de Nice', 'Mont-Boron', 2001, '+165744321', 195);

INSERT INTO Patrons (Id, PrénomNom, Emploi, DateNaissance, Adresse) VALUES (1, 'Jean Paul', 'professeur', TO_DATE('1984/09/12 12:04:06', 'yyyy/mm/dd hh:mi:ss'),  '124 Rue de Belleville 75020 Paris');
INSERT INTO Patrons (Id, PrénomNom, Emploi, DateNaissance, Adresse) VALUES (2, 'Gabriel Martin', 'avocat', TO_DATE('1975/11/25 12:04:06', 'yyyy/mm/dd hh:mi:ss'), '10 Rue Claudia 69002 Lyon');
INSERT INTO Patrons (Id, PrénomNom, Emploi, DateNaissance, Adresse) VALUES (3, 'Julie Durand', 'manager', TO_DATE('1994/03/15 12:04:06', 'yyyy/mm/dd hh:mi:ss'), '3 Quai des Deux Emmanuels 06300 Nice');

INSERT INTO Chiens(Id, IdClub, IdPatron, ChienNom, Race,AnnéeNaissance, Prix) VALUES (1, 1, 1,  'Superstar', 'Carlin', 2012, 1600);
INSERT INTO Chiens(Id, IdClub, IdPatron, ChienNom, Race,AnnéeNaissance, Prix) VALUES (2, 2, 2,  'Noisette', 'Akita Inu', 2018, 2000);
INSERT INTO Chiens(Id, IdClub, IdPatron, ChienNom, Race,AnnéeNaissance, Prix) VALUES (3, 3, 3,  'Princesse', 'Rauque', 2016, 1400);

INSERT INTO Competitions(Id, CompetitionNom, CompetitionDate, Frais, NombreAudience) VALUES (1, 'Competition de Paris', TO_DATE('2020/03/15 12:04:06', 'yyyy/mm/dd hh:mi:ss'), 200, 540);
INSERT INTO Competitions(Id, CompetitionNom, CompetitionDate, Frais, NombreAudience) VALUES (2, 'Competition de Lyon',  TO_DATE('2020/05/25 12:04:06', 'yyyy/mm/dd hh:mi:ss'), 300, 215);
INSERT INTO Competitions(Id, CompetitionNom, CompetitionDate, Frais, NombreAudience) VALUES (3, 'Competition de Nice',  TO_DATE('2020/09/09 12:04:06', 'yyyy/mm/dd hh:mi:ss'), 800, 1000);

INSERT INTO Prix(Id, IdChien, IdCompetition, Prix) VALUES (1, 1, 1, 'Coupe');
INSERT INTO Prix(Id, IdChien, IdCompetition, Prix) VALUES (2, 2, 2, 'Argent');
INSERT INTO Prix(Id, IdChien, IdCompetition, Prix) VALUES (3, 3, 3, 'Réduction au club');

INSERT INTO ChiensCompetitions(IdCompetition, IdChien) VALUES (1,1);
INSERT INTO ChiensCompetitions(IdCompetition, IdChien) VALUES (2,2);
INSERT INTO ChiensCompetitions(IdCompetition, IdChien) VALUES (3,3);

-- AFFICHER DES DONNÉES STOCKÉES DANS LES TABLES

SELECT * FROM Clubs;
 
SELECT * FROM Patrons;
 
SELECT * FROM Chiens;
 
SELECT * FROM Competitions;
 
SELECT * FROM Prix;

SELECT * FROM ChiensCompetitions;

---PROCÉDURES REGROUPÉES DANS UN PAQUETAGE

--Définition de la spécification du paquetage
CREATE OR REPLACE PACKAGE mes_procedures AS  
PROCEDURE Club_Info(IdClub NUMBER);
    	PROCEDURE Augmenter_Frais (idClub NUMBER, somme NUMBER);
    	PROCEDURE Supprimer_Ligne(table_nom IN VARCHAR, condition IN      
    	VARCHAR DEFAULT NULL);
    	PROCEDURE Ajouter_Nouveau_Chien
   	  (chien_id Chiens.id%TYPE, 
  	  club_id Chiens.IdClub%TYPE, 
   	  patron_id Chiens.IdPatron%TYPE, 
    	nouveau_nom Chiens.ChienNom%TYPE, 
   	  nouvelle_race Chiens.Race%TYPE, 
    	date_naissance Chiens.AnnéeNaissance%TYPE, 
    	chien_prix Chiens.Prix%TYPE);  
   	  PROCEDURE Ajouter_Colonne(nom_table VARCHAR);
    	PROCEDURE Top_Prix (k NUMBER);
END mes_procedures;
/
--Définition du corps du paquetage
CREATE OR REPLACE PACKAGE BODY mes_procedures AS
--Procédure qui affiche l`information sur le club par id
PROCEDURE Club_Info(IdClub NUMBER) IS
--Déclaration des variables et exception
      info_club Clubs%ROWTYPE;
      i Number;
      Id_non_existant exception;
BEGIN
--Vérifier l`existence du club id, si la clé primaire n`existe pas lever une exception Id_non_existante
     SELECT COUNT(*) INTO i FROM Clubs Where id = IdClub;
      IF i = 0 THEN
            RAISE Id_non_existant;
      END IF;
--Sélectionner des données de la table par id
      SELECT * INTO info_club FROM Clubs WHERE id = IdClub;
--Affichage écran
      DBMS_OUTPUT.enable;
      DBMS_OUTPUT.put_line(info_club.id);
      DBMS_OUTPUT.put_line(info_club.ClubNom);
      DBMS_OUTPUT.put_line(info_club.Quartier);
      DBMS_OUTPUT.put_line(info_club.AnnéeCréation);
      DBMS_OUTPUT.put_line(info_club.Telephone);
      DBMS_OUTPUT.put_line(info_club.FraisInscription);
--Gestion des exception, si la clé primaire n`existe pas
EXCEPTION
      WHEN Id_non_existant THEN
          RAISE_APPLICATION_ERROR(-20500, 'Club id n`a pas ete trouvé');
END;
--Procédure qui augmente des frais d`inscription au club du montant indiqué comme somme 
PROCEDURE Augmenter_Frais (idClub NUMBER, somme NUMBER) IS 
--Déclaration des variables et exceptions
      i Number;
      cnt NUMBER;
      c_nom VARCHAR(50);
      Id_non_existant exception;
BEGIN
--Vérifier si la clé primaire existe
       SELECT COUNT(*) INTO i FROM Clubs Where id = idClub;
    	 IF i = 0 THEN
        		RAISE Id_non_existant;
    	 END IF;
--Mise à jour des frais d`inscription
     	 UPDATE clubs SET FraisInscription =  FraisInscription + somme WHERE id = IdClub;
       SELECT  FraisInscription, ClubNom INTO cnt, c_nom FROM Clubs WHERE id = IdClub;
--Affichage écran des nouveux frais d`inscription
    	 DBMS_OUTPUT.enable;
       DBMS_OUTPUT.put_line('Nouveau frais d`inscription pour ' || c_nom || ' est ' ||  cnt || ' euros');
--Gestion des exceptions
EXCEPTION
       WHEN Id_non_existant THEN RAISE_APPLICATION_ERROR(-20500, 'Club id n`a pas ete trouvé');
END;
--Procédure qui supprime la ligne de la table indiquée
PROCEDURE Supprimer_Ligne(table_nom IN VARCHAR, condition IN VARCHAR DEFAULT NULL) IS
      where_clause VARCHAR2(100) := ' WHERE ' || condition;
  	  v_table  VARCHAR2(30);
BEGIN
-- Vérification si la table existe
  	  SELECT OBJECT_NAME INTO v_table FROM USER_OBJECTS WHERE OBJECT_NAME = UPPER(table_nom) AND OBJECT_TYPE = 'TABLE';
  	  IF condition IS NULL THEN 
          where_clause := NULL; 
  	  END IF;
--Requete dymanique pour supprimer la ligne de la table en se basant sur la condition indiquée
  	  EXECUTE IMMEDIATE 'DELETE FROM ' || v_table || where_clause;
--Gestion des exceptions
EXCEPTION
      WHEN NO_DATA_FOUND THEN
--Affichage écran
        DBMS_OUTPUT.PUT_LINE ('Table pas valide: ' || table_nom);
END;
--Procédure qui ajoute l`information sur un nouveau chien dans la table
PROCEDURE Ajouter_Nouveau_Chien (
    	chien_id Chiens.id%TYPE, 
    	club_id Chiens.IdClub%TYPE, 
    	patron_id Chiens.IdPatron%TYPE, 
    	nouveau_nom Chiens.ChienNom%TYPE, 
    	nouvelle_race Chiens.Race%TYPE, 
    	date_naissance Chiens.AnnéeNaissance%TYPE, 
    	chien_prix Chiens.Prix%TYPE)   IS 
 	    i Number;
    	cle_pas_unique exception;
BEGIN  
--Vérification si la clé primaire est utilisée
    	SELECT COUNT(*) INTO i FROM Chiens Where id = chien_id;
    	IF i != 0 THEN
        		RAISE cle_pas_unique;
    	END IF;
--Insértion des donnees dans la table
      INSERT INTO Chiens (Id, IdClub, IdPatron, ChienNom, Race, AnnéeNaissance, Prix)  VALUES (chien_id, club_id, patron_id, nouveau_nom, nouvelle_race, date_naissance, chien_prix); 
--Affichage écran
      DBMS_OUTPUT.put_line('Nouveau chien ' || nouveau_nom || ' a été ajouté dans la table');
--Gestion des exceptions;
EXCEPTION
      WHEN cle_pas_unique THEN RAISE_APPLICATION_ERROR(-20500, 'Chien id n`est pas unique!');
END;
--Procédure qui ajoute une nouvelle colonne 'Ville' avec une requete dynamique    
PROCEDURE Ajouter_Colonne(nom_table VARCHAR) IS
    	requete VARCHAR(50);
BEGIN
--Requete dynamique. Requete est exécutée en mode dynamique parce qu`elle altère la structure de la base de données
    	requete := 'ALTER TABLE ' || nom_table || ' ADD  Ville VARCHAR(30)';
    	EXECUTE IMMEDIATE requete;
--Gestion des exceptions
EXCEPTION
      WHEN NO_DATA_FOUND THEN
         DBMS_OUTPUT.PUT_LINE ('Table pas valide: ' || nom_table);
 END;
--Procédure qui affiche les top 3 chiens par prix décroissant
PROCEDURE Top_Prix (k Number)  IS
 --Création du curseur
CURSOR ranking IS SELECT ChienNom, Prix FROM Chiens ORDER BY prix DESC;
    	n ranking%ROWTYPE;
BEGIN
    	OPEN ranking;
--Premiere ligne
      FETCH ranking INTO n;
      WHILE ranking%FOUND AND ranking%ROWCOUNT <= 3 LOOP
          DBMS_OUTPUT.PUT_LINE(ranking%ROWCOUNT || ')' || n.ChienNom || ' : ' || n.prix || ' USD');
--Ligne suivante
        	FETCH ranking INTO n;
    	END LOOP;
    	CLOSE ranking;
END;
END mes_procedures;
/
--Vérification d`éxecution

execute mes_procedures.Club_Info(1);
execute mes_procedures.Augmenter_Frais(1, 100); 
execute mes_procedures.Supprimer_Ligne('Prix', 'id = 1'); 
execute mes_procedures.Ajouter_Nouveau_Chien(5, 3, 2, 'King', 'shepherd', 2001, 500);
execute mes_procedures.Ajouter_Colonne('Competitions');
execute mes_procedures.Top_Prix(3);


--DÉCLENCHEUR AVANT INSERTION
--Création du déclencheur qui vérifie l`age du chien avant l`insertion des données. 
--Si le chien est trop jeune, son age est égal ou moins de 2 ans - il ne peut pas joindre le club. Il est impossible d`insérer l`information sur ce chien.
CREATE OR REPLACE TRIGGER Vérifier_Age
      BEFORE INSERT 
      ON Chiens
      FOR EACH ROW
      DECLARE
--Déclaration des variables et exceptions
      i NUMBER;
      annee NUMBER;
      cle_nulle EXCEPTION;
      cle_utilisee EXCEPTION;
      age_pas_valide EXCEPTION;
BEGIN
--Vérification si la clé primaire est vide
      IF :NEW.id IS NULL THEN
          RAISE cle_nulle;
      END IF;
--Vérification si la clé primaire est déjà utilisée
      SELECT COUNT(*) INTO i FROM Chiens WHERE id = :NEW.id;
      IF i > 0 THEN
          RAISE cle_utilisee;
      END IF;
--Vérification si l`age du chien n`est pas nulle et correspond aux exigences--         
      SELECT AnnéeNaissance INTO annee FROM Chiens WHERE AnnéeNaissance =:NEW.AnnéeNaissance;
      IF annee >= 2018 THEN
            RAISE age_pas_valide;
      END IF;
--Gestion des exceptions
EXCEPTION
        WHEN cle_nulle THEN RAISE_APPLICATION_ERROR(-20501, 'Une clé primaire doit avoir une valeur !');
        WHEN cle_utilisee THEN RAISE_APPLICATION_ERROR(-20502, 'Clé primaire est déjà utilisée !');
        WHEN age_pas_valide THEN RAISE_APPLICATION_ERROR(-20504, 'Le chien est trop jeune pour joindre le club!');
END;
/

--Vérification d`éxecution
INSERT INTO Chiens(Id, IdClub, IdPatron, ChienNom, Race,AnnéeNaissance, Prix) VALUES (4, 1, 1,  'Superstar', 'Carlin', 2018, 1600);
INSERT INTO Chiens(Id, IdClub, IdPatron, ChienNom, Race,AnnéeNaissance, Prix) VALUES (1, 1, 1,  'Superstar', 'Carlin', 2018, 1600);
INSERT INTO Chiens(Id, IdClub, IdPatron, ChienNom, Race,AnnéeNaissance, Prix) VALUES (null, 1, 1,  'Superstar', 'Carlin', 2016, 1600);
INSERT INTO Chiens(Id, IdClub, IdPatron, ChienNom, Race,AnnéeNaissance, Prix) VALUES (4, 1, 1,  'Superstar', 'Carlin', 2016, 1600);
