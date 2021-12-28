Τεχνολογικό Εκπαιδευτικό Ίδρυμα Αθήνας Τμήμα Μηχανικών Πληροφορικής Τ.Ε Προγραμματισμός Υπολογιστών (Εργαστήριο ) Εργασία 07 ![](Aspose.Words.81d907b5-5c15-4008-8525-1d524e877970.001.png)

Στέφανος Στεφάνου Αριθμός Μητρώου : 161118 Τμήμα Α3(Τετάρτη 8:00-10:00) 

*\*\*Τα παρακάτω προγράμματα γραφτήκανε και δοκιμαστήκανε κάτω απο ZorinOS 64bit(Ubuntu based) και gcc έκδοση 5.4.0 κάτω απο το πρότυπο c99(ISO/IEC 9899:1999)\*\**

*Άσκηση 1)*

Να γραφεί πρόγραμμα το οποίο θα διαβάζει τα στοιχεία Ν φοιτητών από την standard είσοδο και στην συνέχεια θα τα γράφει σε αρχείο κειμένου. 

*Κώδικας Άσκησης 1)*

*#include <stdio.h>* 

#include <stdlib.h> 

#include <memory.h> 

#define FAIL NULL 

#define SUCCESS 1 

#define ERR 2 

#define FOPEN\_ERROR -1 

/\* 

* Συνάρτηση openfile()
* Ανοίγει το αρχείο σε κατάσταση append ("a")
* @:arg
* δεν λαµβάνει καµια παράµετρο
* @:returns
* NULL σε περίπτωση αποτυχίας ανοίγµατος αρχείου
* εναν δείκτη σε δεδοµένα τύπου FILE (struct) assosiated µε ενα αρχείο , 

το οποίο δείχνει στο τέλος (κατάσταση append)

\*/

FILE \*openfile() { **return** fopen("data.txt","a"); } 

/\* 

* Συνάρτηση writeintoopenedfile(FILE \*\*,char \*\*)
* Γράφει πάνω στο αρχείο /Stream το οποίο δίνεται ως πρώτη παράµετρος , τα 

δεδοµένα που δίνονται στον buffer (δευτερη παράµετρος)

* Προσοχή! πρέπει να έχει ήδη ανοιχτεί το αρχειο /Stream πριν την κλήση της 

writeintofile και να εχε δεσµευτεί δυναµικα

* ο buffer
* @:arg
* FILE \*\* file\_descriptor -> Εναν pointer προς pointers τύπου FILE
* char \*\* Buffer -> η περιοχή ενδιάµεσης µνήµης προς εγραφή
* @:returns ακαίρεο (int)
* SUCCESS (1) Σε περίπτωση επιτυχίας
* ERR(2) Σε διαφορετική περίπτωση

\*/

**int** writeintoopenedfile(FILE \*\*file\_descriptor,**char** \*\*Buffer){ 

fflush(\*file\_descriptor);

**if**(fprintf(\*file\_descriptor,\*Buffer)){

**return** SUCCESS;

}

**else return** ERR;

} 

/\* 

* Συνάρτηση ΙΝΙΤ\_ΜΕΜΟΡΥ(char \*\*Buffer)
* Κάνει τις απαραίτητες δεσµεύσεις στην µνήµη για την περιοχή ενδιάµεσης 

αποθήκευσης Buffer

* @:arg
* char \*\* Buffer , εναν διπλο pointer προς την περιοχή ενδιάµεσης 

αποθήκευσης

* @:no\_returns (void)

\*/

**void** INIT\_MEMORY(**char** \*\*Buffer){ 

\*Buffer=(**char** \*)malloc(200\***sizeof**(**char**));

**if**(\*Buffer==FAIL){

fprintf(stderr,"cant allocate memory!");

exit(EXIT\_FAILURE);

}

memset(\*Buffer,0,200\***sizeof**(**char**));

} 

/\* 

* Σύνάρτηση EXIT\_PROCCESS(File \*\*file\_pointer,char \*\*Buffer)
* Κάνει την απαραίτηση απελευθέρωση µνήµης και κλείνει το ρεύµα εξόδου που 

ανοίχτηκε µε την openfile()

* @:arg
* FILE \*\*file\_pointer -> ενα ανοιχτό stream προς αρχείο
* char \*\*Buffer -> εναν pointer που δείχνει σε δυναµικα δεσµευµένο 

πίνακα!!!!

* @:no\_returns!

\*

* @:warn
* το ρεύµα θα πρέπει να είναι ανοιγµένο µε µια κληση της openfile()
* Η µνήµη πρέπει να είναι δυναµικά δεσµευµένη µε µια κλήση της 

INIT\_MEMORY

\*

\*/

**void** EXIT\_PROCCESS(FILE \*\*file\_pointer,**char** \*\*Buffer){ 

fclose(\*file\_pointer);

free(\*Buffer);

} 

**void** Call\_UI(**char** \*\*Buffer,FILE \*\*file\_pointer){ 

**int** num,i;

size\_t buffsize=200;//Δήλωση µεγέθους buffer (το size\_t είναι ενα εσωτερικό typedef της c -> unsigned int)

printf("Δώσε αριθµό µαθητών");

scanf("%d",&num);

**for** (i = 0; i <=num; ++i) {

**if**(i==0){

fflush(\*file\_pointer);

getline(Buffer,&buffsize,stdin);

**continue**;

}

printf("%d)Δώσε στοιχεία φοιτητη\n",i);

fflush(stdin);

getline(Buffer,&buffsize, stdin); **if**(writeintoopenedfile(file\_pointer,Buffer)==SUCCESS){

printf("%s -> [Success into data.txt]\n",\*Buffer);

}

**else**{

printf("%s -> [Error into data.txt\n",\*Buffer);

}

}

} 

**int** main() { 

**char** \*Buffer;                              //Δήλωση Buffer

FILE \*file\_pointer=openfile();            //Άνοιγµα αρχείου

INIT\_MEMORY(&Buffer);                    //Αρχικοποίηση Μνήµης

Call\_UI(&Buffer,&file\_pointer);         //Κλήση συνάρτησης που υλοιποιεί το UserInterface

EXIT\_PROCCESS(&file\_pointer,&Buffer);  //Εξοδος

} 

*Άσκηση 2)*

Να γραφεί πρόγραμμα το οποίο θα διαβάζει από αρχείο κειμένου τα στοιχεία όλων των φοιτητών που βρίσκονται σε αυτό και στη συνέχεια θα τυπώνει στην standard έξοδο τα στοιχεία του φοιτητή με τον μεγαλύτερο μέσο όρο. *#include <stdio.h>* 

#include <stdlib.h> 

#include <memory.h> 

#define FAIL NULL 

#define SUCCESS 1 

#define ERR 2 

#define AM\_Size 6 

#define NAME\_Size 40 

#define GIVE\_ME\_MAX 1 

/\* 

* Struct Record
* Αποθηκεύει τα δεδοµένα του κάθε φοιτητη

\*

\*/

**struct** record{ 

**char** AM[AM\_Size+1];

**char** NAME[NAME\_Size+1];

**char** SURNAME[NAME\_Size+1];

**int** semester;

**int**  Marks[5];

}; 

/\* 

* Συνάρτηση opefile
* Ανοίγει το αρχείο data.txt προς αναγνωση (reading mode "r")

\*

* @:no\_args

\*

* @returns
* FAIL(NULL) σε περίπτωση αποτυχίας
* FILE \* fp -> εναν file pointer που δείχνει το αρχειο data.txt

\*

\*/

FILE \*openfile(){ 

FILE \*fp = fopen("data.csv","r");

**if**(fp==FAIL){

fprintf(stderr,"[CantOpenFileException :P -> ]Cant open file,check if exists,give me sudo permissions maybe? :P ");

**return** FAIL;

}

**else return** fp;

} 

/\* 

* Συνάρτηση EXIT\_PROCCESS(FILE \*\*,char \*\*)
* Αποδεσµεύει τον δυναµικά δεσµευµένο Buffer και κλείνει το ρεύµα εξοδου
* @:arg
* Εναν δείκτη σε δείκτες τύπου FILE -> το ανοιχτό ρεύµα εξόδου που 

πρόκειται να κλείσει

* Εναν δείκτη προς τον Buffer Εισόδου -> ο Buffer που θα αποδεσµευτεί ! \*/

**void** EXIT\_PROCCESS(FILE \*\*file\_pointer,**char** \*\*Buffer){ 

fclose(\*file\_pointer);

free(\*Buffer);

} 

/\* 

* Συνάρτηση CalculateMo(struct record \*TempRawData)
* @:returns double
* Τον αριθµο που αντιπροσωπεύει τον µέσο όρο !
* @:args
* struct record \*TempRawData -> ενας δείκτης σε δοµή που περιέχει 

στοιχεία φοιτητή

\*

\*/

**double** CalculateMO(**struct** record \*TempRawData){ 

**int** i,temp=0;

**for**(i=0;i<5;i++){

temp+=TempRawData->Marks[i];

}

**return** temp/5;

} 

/\* 

* Συνάρτηση HandleStudentRawData(struct record \*,struct record \*)
* Χειρίζεται µια νεά εισαγωγή δεδοµένων απο το αρχείο σε µορφη struct record
* Ελένχει αν είναι ο µεγαλυτερος µεσος όρος µεχρι στιγµης και προβαίνει στις 

κατάλληλες ενεργειες

\*

\*

* @:no\_return

\*

\*

* @:args
* struct record \*TempRawData -> ενας δείκτης σε δοµή που περιέχει 

στοιχεία φοιτητή

* struct record \*MaxStudent -> ενας δείκτης σε δοµή η οποία περιέχει τα 

στοιχεία του καλύτερου φοιτητη µεχρι στιγµής

\*

\*/

**void** HandleStudentRawData(**struct** record \*TempRawData,**struct** record \*MaxStudent){ 

**static double** maxMO=0;

**double** tempMO=CalculateMO(TempRawData);

**if**(tempMO>maxMO){

maxMO=tempMO;

\*MaxStudent=\*TempRawData;

}

} 

/\* 

* Συνάρτηση show\_results (struct record \*)

\*

* Εκτυπωνει τα στοιχεία ενος φοιτητή

\*

* @:arg
* struct record \*PrintRecord -> τα στοιχεία ενος φοιτητή προς εκτύπωση!
* @:no\_return

\*/

**void** show\_results(**struct** record \*PrintRecord){ 

**int** i;

printf("ID : %s\n",PrintRecord->AM);

printf("NAME : %s\n",PrintRecord->NAME);

printf("SURNAME : %s\n",PrintRecord->SURNAME);

printf("SEMESTER : %d\n",PrintRecord->semester);

**for**(i=0;i<5;i++){

printf("LESSON %d WITH MARK %d\n",i,PrintRecord->Marks[i]);

}

} 

/\* 

* Συνάρτηση INIT\_MEMORY(char \*\* )
* Αρχικοποιεί τον Buffer εισόδου (200 bytes)
* @:args
* char \*\*Buffer -> ο δείκτης που θα δείχνει στον buffer εισόδου
* @:returns
* FAIL(NULL) -> Σε περίπτωση σφαλµατος στην δεσµεύση χώρου
* SUCCESS(1)  ->Σε περίπτωση επιτυχίας αρχικοποιησης

\*/

**int** INIT\_MEMORY(**char** \*\*Buffer,**struct** record \*MaxStudent,**struct** record \*TempStudent){ 

\*Buffer = (**char** \*)malloc(**sizeof**(**char**)\*250);

**if**(\*Buffer==NULL){

**return** FAIL;

}

memset(\*Buffer,0,250);

memset(MaxStudent,0,**sizeof**(**struct** record)); memset(TempStudent,0,**sizeof**(**struct** record));

} 

**int** main(){ 

**struct** record MaxStudent,TempStudent;               //Δηµιουργία µεταβλητών -δοµών για µεγιστο και προσωρινά δεδοµένα

**int** maxMarkMO=0,num,i;                              //Μεγιστος µεσος όρος,αριθµος γραµµών και µετρητής βρόνχου

**char** \*Buffer;                                       //pointer στον Buffer

INIT\_MEMORY(&Buffer,&MaxStudent,&TempStudent);      //Αρχικοποίηση µεταβλητών

FILE \*file\_pointer=openfile();                      //Δηµιουργια του file\_pointer

size\_t BufferSize=250;                              //Μέγεθος Buffer

fscanf(file\_pointer,"%d\n",&num);                   //Διάβασµα γραµµών απο το αρχειο και αγνόηση ενος \n

**for**(i=0;i<num;i++){

fflush(stdin);

getline(&Buffer,&BufferSize,file\_pointer);      //Φόρτωση στην µνήµη της κάθε γραµµης

//Ξεδιάλεξη Δεδοµένων sscanf(Buffer,"%6[^,],%s %[^,],%d,%d,%d,%d,%d,%d",TempStudent.AM,TempStudent.NAME,TempStudent.SURNAME,&T empStudent.semester,&TempStudent.Marks[0],&TempStudent.Marks[1],&TempStudent.Ma rks[2],&TempStudent.Marks[3],&TempStudent.Marks[4]);

HandleStudentRawData(&TempStudent,&MaxStudent); //Σύγριση των δεδοµένων }

show\_results(&MaxStudent); //Εκτύπωση µεγαλύτερου

} 
