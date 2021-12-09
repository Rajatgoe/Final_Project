# FINALCRICKET WEBSCRAPING PROJECT


![image](https://github.com/800db810b58fd380a6081ff35d6ae30848725ba0/246097.png)



### Cricket is a sport that is held in following three formats namely – T20(Twenty 20), ODI (One Day International) and Test Matches. All these formats pose a very different challenge for the players to showcase their talents and abilities. Cricket being a very popular sport has encouraged countries to bring in their best talentedplayer. This particular fact is the very reason why scrapping the individual player’s record of top ODI (One Day International) batsman and bowler of every country makes a statement for a countries progress in this particular sport.***

### The goal of the final project is to scrap the data from the website adderss-("https://stats.espncricinfo.com/ci/engine/records/index.html") about the cricket.In this project, I scraped the data about top batsman and bowler and compare their performance through SQL query. Furthermore, the comparison will be based on their total run scored by the batsman in their batting career and maximum wicket had been taken by the bowler in their bowling carrier!

# The main task of this project was to scrap the data from two differant pages and to structure it in the list of dictionaries. 

# At first I have requested to import the data from url = 'https://stats.espncricinfo.com/ci/engine/records/index.html?class=2' and printed the length of alphabet in data which is 129323. json is been used to convert the string into json viewer format and to get the key : value  pairs.
import requests,json 

url = 'https://stats.espncricinfo.com/ci/engine/records/index.html?class=2'
r = requests.get(url)
#print(len(r.text))   # for printing amount of charater(alphabets)

# Next, I have splitted the lines  and used print(len(lines)) function to get the print of number of line in the main page which is 1675. 

#lines = r.text.split('\n')   # distributing the lines
#print(len(lines))   # no. of lines
                                           # It is a raw HTML string
                                           
# Here, I have scraped the data using view source page  of https://stats.espncricinfo.com/ci/engine/records/index.html?class=2, which includes the data of all the top batsman and bowler using split method . I have assigned an Variable for each row(rc) and column(cc) to get the exact location of the URL, then split it for exact locatio of data and this helps me to find out the URL of top batsman and top bowlers. Also I had taken first step to structure of data by opening an dictionary name data = {}.

total = r.text.split( 'a name="batting">')[1].split('<a class="RecordLinks" href="/ci/content/records/283204.html')[0]   
lines = total.split('\n')   #splitted each line same as view source page
data = {}
rc = 0
cc = 0
n = 0

cricket = []
for line in lines:
    cricketer = {}
   
    if '<li' in line:     # for putting rows            #(1)
        rc+=1
        cc = 0        # column count need to be reset at the end of the row
    if '<a' in line:
        cc+=1
    
    if ' </li>' in line:  # for removing the </li>  (its an anamolous row)      #(3)
        cc=0        
    
#####for runs 
                

    if cc == 1 and rc == 1:                       #(2)   #for taking out batsman run url
        batsman = line.split("RecordLinks")[1].split(">Most runs")[0]   #use n for printing the line         cricketer['batsmanODI'] = batsman                   with number in increasing order or use                                                               rc for row cound and cc for column count
                                                       # splitting for getting the url from the line
        
##############################################################################################        
        url1 = line.split('"')[3]       # first splitted by using quotation(") mark then taken third                                            element which is the URL and uesd in next line
        #print(url)# dcds"sdsdd"ddddedd
        url1 = 'https://stats.espncricinfo.com'+ url1
        r = requests.get(url1) 

        totals = r.text.lower().split("<tbody>")
        
        lines = totals[1].split('\n')
        #print(totals[1])
        prc =1
        pcc =1
        players = []     ################   nested list of dictionary--larger basket
        player = {}  #   ------- smaller basket
        n=0
        for line in lines:
                  ################  dictionary inside nested list 
            if '<td' in line:
                pcc+=1
            if '<tr' in line:
                prc +=1
            if '</tr>' in line:
                pcc =1
        

              
            if pcc == 2 and '<a href' in line:
                Batsman = line.upper().split(">")[2].split("</A")[0]    # total[0] is before ("<tbody>") and total[1] is after ("<tbody>")
                #print(batsman)
                
                player["Batsman_name"] = Batsman
               #players.append(player) 
            if pcc == 4 and '<td class' in line : 
                Matches = line.split(">")[1].split("</td")[0]  
                player["Batsman_matches"] = Matches
                
            if pcc == 7 and '<td nowrap' in line : 
                Runs = line.split("<b>")[1].split("</b>")[0]
               
           
                player["Runs_scored"] = Runs
                
                players.append(player) 
                player = {} # to clear to player object  -- for each line it will dump into list[players
        cricketer["batsman_detail"] = players
        cricket.append(cricketer)
                    
                #print("(Batsman Name) -", Batsman,"(Total Runs In Career) -", Runs, "\n")
### Above, after inserting in the batsman URL "https://stats.espncricinfo.com+ url1( which is equal to ---   /ci/content/records/83548.html") through scraping from the main page which includes the data of all the top batsman using split method. I have assigned an Variable for each row(prc) and column(pcc) to get the exact data of batsman name, total matches he played and total runs he had scored in career.              
    ##############################  BOWLERS              
    if cc ==1 and rc == 74:                       # for taking out bowlers wicket url
        bowler = line.split("RecordLinks")[1].split(">Most wickets")[0]
                          # splitting for getting the url from the line
            
        cricketer['bowlerODI'] = bowler
        
        url1 = line.split('"')[3]    # first splitted by using quotation(") mark then taken third element which is the URL and uesd in next line
        #print(url)# dcds"sdsdd"ddddedd
        url1 = 'https://stats.espncricinfo.com'+ url1
        r = requests.get(url1) 

        totals = r.text.lower().split("<tbody>")

        lines = totals[1].split('\n')
        #print(totals[1])
        pprc =1
        ppcc =1
        players1 = []     ################   nested list of dictionary
        player1 = {}      ################  dictionary inside nested list 
        n = 0
        for line in lines:
            
            if '<td' in line:
                ppcc+=1
            if '<tr' in line:
                pprc +=1
            if '</tr>' in line:
                ppcc =1
            

            #print(prc, pcc, line)

            if ppcc == 2 and '<a href' in line:
                Bowler = line.upper().split(">")[2].split("</A")[0]    # total[0] is before ("<tbody>") and total[1] is after ("<tbody>")
                
                player1["Bowler_name"] = Bowler
            
               #players1.append(player1) 
                
                
                #print(Bowler)
            if ppcc == 4 and '<td class' in line : 
                Matches = line.split(">")[1].split("</td")[0]  
                player1["Bowler_matches"] = Matches
        ##      
            if ppcc == 8 and '<td nowrap' in line: 
                Wicket = line.split("<b>")[1].split("</b>")[0]
                      
                player1["Wicket_taken"] = Wicket
                players1.append(player1) 
                
             
                player1 = {}
             
### Above, after inserting in the bowler URL "https://stats.espncricinfo.com+ url1( which is equal to ---   "/ci/content/records/283193.html") through scraping from the main page which includes the data of all the top bowler using split method. I have assigned an Variable for each row(pprc) and column(ppcc) to get the exact data of bowler name, total matches he played and total wicket he had taken in career.                
                
            
          
        cricketer["bowler_detail"] = players1
        cricket.append(cricketer)
#  As the last, I did data structure using list of dictionaries (cricket = []) and  append each loop into into list of dictionaries. First I have added each loop of dictionary into seperate list then added nested list of dictionary into main(cricket = []) list of dictionaries.                   
                #print("(Bowlers Name) -", Bowler,"(Total Wickets In Career) -", Wicket, '\n')
                #print(Bowler, Wicket)
                
                
                #print("(Batsman Name) -", Batsman,"(Total Runs In Career) -", Runs, "\n",)
                #print("(Bowlers Name) -", Bowler,"(Total Wickets In Career) -", Wicket, '\n')
                     
                                
    n+=1
                                              
# First I printed the data in json format and then because the data changes on the website, I dump the data in file cricket.json( Open the new file(cricket.json) in write mode, then transferred the output of the code and closed the file(cricket.json had been saved in the same directory.)
print(json.dumps(cricket))
f = open('cricket.json','w')
f.write(json.dumps(cricket))
f.close()   

#print(cricket)  
 
### OUTPUT - Has a list of dictionaries which represent key : value pair. for example - 
### 0
### Batsman_name : "SR TENDULKAR"
### Batsman_matches : "463"
### Runs_scored : "18426"
### Here 0 is the KEY and "SR TENDULKAR","463", "18426" are PAIRS.

[{"batsmanODI": "\" href=\"/ci/content/records/83548.html\"", "batsman_detail": [{"Batsman_name": "SR TENDULKAR", "Batsman_matches": "463", "Runs_scored": "18426"}, {"Batsman_name": "KC SANGAKKARA", "Batsman_matches": "404", "Runs_scored": "14234"}, {"Batsman_name": "RT PONTING", "Batsman_matches": "375", "Runs_scored": "13704"}, {"Batsman_name": "ST JAYASURIYA", "Batsman_matches": "445", "Runs_scored": "13430"}, {"Batsman_name": "DPMD JAYAWARDENE", "Batsman_matches": "448", "Runs_scored": "12650"}, {"Batsman_name": "V KOHLI", "Batsman_matches": "254", "Runs_scored": "12169"}, {"Batsman_name": "INZAMAM-UL-HAQ", "Batsman_matches": "378", "Runs_scored": "11739"}, {"Batsman_name": "JH KALLIS", "Batsman_matches": "328", "Runs_scored": "11579"}, {"Batsman_name": "SC GANGULY", "Batsman_matches": "311", "Runs_scored": "11363"}, {"Batsman_name": "R DRAVID", "Batsman_matches": "344", "Runs_scored": "10889"}, {"Batsman_name": "MS DHONI", "Batsman_matches": "350", "Runs_scored": "10773"}, {"Batsman_name": "CH GAYLE", "Batsman_matches": "301", "Runs_scored": "10480"}, {"Batsman_name": "BC LARA", "Batsman_matches": "299", "Runs_scored": "10405"}, {"Batsman_name": "TM DILSHAN", "Batsman_matches": "330", "Runs_scored": "10290"}, {"Batsman_name": "MOHAMMAD YOUSUF", "Batsman_matches": "288", "Runs_scored": "9720"}, {"Batsman_name": "AC GILCHRIST", "Batsman_matches": "287", "Runs_scored": "9619"}, {"Batsman_name": "AB DE VILLIERS", "Batsman_matches": "228", "Runs_scored": "9577"}, {"Batsman_name": "M AZHARUDDIN", "Batsman_matches": "334", "Runs_scored": "9378"}, {"Batsman_name": "PA DE SILVA", "Batsman_matches": "308", "Runs_scored": "9284"}, {"Batsman_name": "RG SHARMA", "Batsman_matches": "227", "Runs_scored": "9205"}, {"Batsman_name": "SAEED ANWAR", "Batsman_matches": "247", "Runs_scored": "8824"}, {"Batsman_name": "S CHANDERPAUL", "Batsman_matches": "268", "Runs_scored": "8778"}, {"Batsman_name": "YUVRAJ SINGH", "Batsman_matches": "304", "Runs_scored": "8701"}, {"Batsman_name": "DL HAYNES", "Batsman_matches": "238", "Runs_scored": "8648"}, {"Batsman_name": "LRPL TAYLOR", "Batsman_matches": "233", "Runs_scored": "8581"}, {"Batsman_name": "MS ATAPATTU", "Batsman_matches": "268", "Runs_scored": "8529"}, {"Batsman_name": "ME WAUGH", "Batsman_matches": "244", "Runs_scored": "8500"}, {"Batsman_name": "V SEHWAG", "Batsman_matches": "251", "Runs_scored": "8273"}, {"Batsman_name": "HM AMLA", "Batsman_matches": "181", "Runs_scored": "8113"}, {"Batsman_name": "HH GIBBS", "Batsman_matches": "248", "Runs_scored": "8094"}, {"Batsman_name": "SHAHID AFRIDI", "Batsman_matches": "398", "Runs_scored": "8064"}, {"Batsman_name": "SP FLEMING", "Batsman_matches": "280", "Runs_scored": "8037"}, {"Batsman_name": "MJ CLARKE", "Batsman_matches": "245", "Runs_scored": "7981"}, {"Batsman_name": "EJG MORGAN", "Batsman_matches": "246", "Runs_scored": "7701"}, {"Batsman_name": "TAMIM IQBAL", "Batsman_matches": "219", "Runs_scored": "7666"}, {"Batsman_name": "SR WAUGH", "Batsman_matches": "325", "Runs_scored": "7569"}, {"Batsman_name": "SHOAIB MALIK", "Batsman_matches": "287", "Runs_scored": "7534"}, {"Batsman_name": "A RANATUNGA", "Batsman_matches": "269", "Runs_scored": "7456"}, {"Batsman_name": "JAVED MIANDAD", "Batsman_matches": "233", "Runs_scored": "7381"}, {"Batsman_name": "YOUNIS KHAN", "Batsman_matches": "265", "Runs_scored": "7249"}, {"Batsman_name": "SALEEM MALIK", "Batsman_matches": "283", "Runs_scored": "7170"}, {"Batsman_name": "NJ ASTLE", "Batsman_matches": "223", "Runs_scored": "7090"}, {"Batsman_name": "GC SMITH", "Batsman_matches": "197", "Runs_scored": "6989"}, {"Batsman_name": "WU THARANGA", "Batsman_matches": "235", "Runs_scored": "6951"}, {"Batsman_name": "MJ GUPTILL", "Batsman_matches": "186", "Runs_scored": "6927"}, {"Batsman_name": "MG BEVAN", "Batsman_matches": "232", "Runs_scored": "6912"}, {"Batsman_name": "G KIRSTEN", "Batsman_matches": "185", "Runs_scored": "6798"}, {"Batsman_name": "A FLOWER", "Batsman_matches": "213", "Runs_scored": "6786"}, {"Batsman_name": "IVA RICHARDS", "Batsman_matches": "187", "Runs_scored": "6721"}, {"Batsman_name": "BRM TAYLOR", "Batsman_matches": "205", "Runs_scored": "6684"}, {"Batsman_name": "MOHAMMAD HAFEEZ", "Batsman_matches": "218", "Runs_scored": "6614"}, {"Batsman_name": "SHAKIB AL HASAN", "Batsman_matches": "215", "Runs_scored": "6600"}, {"Batsman_name": "MUSHFIQUR RAHIM", "Batsman_matches": "227", "Runs_scored": "6581"}, {"Batsman_name": "GW FLOWER", "Batsman_matches": "221", "Runs_scored": "6571"}, {"Batsman_name": "IJAZ AHMED", "Batsman_matches": "250", "Runs_scored": "6564"}, {"Batsman_name": "AR BORDER", "Batsman_matches": "273", "Runs_scored": "6524"}, {"Batsman_name": "RB RICHARDSON", "Batsman_matches": "224", "Runs_scored": "6248"}, {"Batsman_name": "KS WILLIAMSON", "Batsman_matches": "151", "Runs_scored": "6173"}, {"Batsman_name": "ML HAYDEN", "Batsman_matches": "161", "Runs_scored": "6133"}, {"Batsman_name": "JE ROOT", "Batsman_matches": "152", "Runs_scored": "6109"}, {"Batsman_name": "S DHAWAN", "Batsman_matches": "145", "Runs_scored": "6105"}, {"Batsman_name": "BB MCCULLUM", "Batsman_matches": "260", "Runs_scored": "6083"}, {"Batsman_name": "DM JONES", "Batsman_matches": "164", "Runs_scored": "6068"}, {"Batsman_name": "DC BOON", "Batsman_matches": "181", "Runs_scored": "5964"}, {"Batsman_name": "JN RHODES", "Batsman_matches": "245", "Runs_scored": "5935"}, {"Batsman_name": "RAMIZ RAJA", "Batsman_matches": "198", "Runs_scored": "5841"}, {"Batsman_name": "AD MATHEWS", "Batsman_matches": "218", "Runs_scored": "5835"}, {"Batsman_name": "RR SARWAN", "Batsman_matches": "181", "Runs_scored": "5804"}, {"Batsman_name": "CL HOOPER", "Batsman_matches": "227", "Runs_scored": "5761"}, {"Batsman_name": "SR WATSON", "Batsman_matches": "190", "Runs_scored": "5757"}, {"Batsman_name": "H MASAKADZA", "Batsman_matches": "209", "Runs_scored": "5658"}, {"Batsman_name": "SK RAINA", "Batsman_matches": "226", "Runs_scored": "5615"}, {"Batsman_name": "MN SAMUELS", "Batsman_matches": "207", "Runs_scored": "5606"}, {"Batsman_name": "WJ CRONJE", "Batsman_matches": "188", "Runs_scored": "5565"}, {"Batsman_name": "F DU PLESSIS", "Batsman_matches": "143", "Runs_scored": "5507"}, {"Batsman_name": "DA WARNER", "Batsman_matches": "128", "Runs_scored": "5455"}, {"Batsman_name": "MEK HUSSEY", "Batsman_matches": "185", "Runs_scored": "5442"}, {"Batsman_name": "IR BELL", "Batsman_matches": "161", "Runs_scored": "5416"}, {"Batsman_name": "A JADEJA", "Batsman_matches": "196", "Runs_scored": "5359"}, {"Batsman_name": "Q DE KOCK", "Batsman_matches": "124", "Runs_scored": "5355"}, {"Batsman_name": "DR MARTYN", "Batsman_matches": "208", "Runs_scored": "5346"}, {"Batsman_name": "G GAMBHIR", "Batsman_matches": "147", "Runs_scored": "5238"}, {"Batsman_name": "AJ FINCH", "Batsman_matches": "132", "Runs_scored": "5232"}, {"Batsman_name": "ADR CAMPBELL", "Batsman_matches": "188", "Runs_scored": "5185"}, {"Batsman_name": "RS MAHANAMA", "Batsman_matches": "213", "Runs_scored": "5162"}, {"Batsman_name": "CG GREENIDGE", "Batsman_matches": "128", "Runs_scored": "5134"}, {"Batsman_name": "MISBAH-UL-HAQ", "Batsman_matches": "162", "Runs_scored": "5122"}, {"Batsman_name": "JP DUMINY", "Batsman_matches": "199", "Runs_scored": "5117"}, {"Batsman_name": "PD COLLINGWOOD", "Batsman_matches": "197", "Runs_scored": "5092"}, {"Batsman_name": "A SYMONDS", "Batsman_matches": "198", "Runs_scored": "5088"}, {"Batsman_name": "ABDUL RAZZAQ", "Batsman_matches": "265", "Runs_scored": "5080"}]}, {"bowlerODI": "\" href=\"/ci/content/records/283193.html\"", "bowler_detail": [{"Bowler_name": "M MURALITHARAN", "Bowler_matches": "350", "Wicket_taken": "534"}, {"Bowler_name": "WASIM AKRAM", "Bowler_matches": "356", "Wicket_taken": "502"}, {"Bowler_name": "WAQAR YOUNIS", "Bowler_matches": "262", "Wicket_taken": "416"}, {"Bowler_name": "WPUJC VAAS", "Bowler_matches": "322", "Wicket_taken": "400"}, {"Bowler_name": "SHAHID AFRIDI", "Bowler_matches": "398", "Wicket_taken": "395"}, {"Bowler_name": "SM POLLOCK", "Bowler_matches": "303", "Wicket_taken": "393"}, {"Bowler_name": "GD MCGRATH", "Bowler_matches": "250", "Wicket_taken": "381"}, {"Bowler_name": "B LEE", "Bowler_matches": "221", "Wicket_taken": "380"}, {"Bowler_name": "SL MALINGA", "Bowler_matches": "226", "Wicket_taken": "338"}, {"Bowler_name": "A KUMBLE", "Bowler_matches": "271", "Wicket_taken": "337"}, {"Bowler_name": "ST JAYASURIYA", "Bowler_matches": "445", "Wicket_taken": "323"}, {"Bowler_name": "J SRINATH", "Bowler_matches": "229", "Wicket_taken": "315"}, {"Bowler_name": "DL VETTORI", "Bowler_matches": "295", "Wicket_taken": "305"}, {"Bowler_name": "SK WARNE", "Bowler_matches": "194", "Wicket_taken": "293"}, {"Bowler_name": "SAQLAIN MUSHTAQ", "Bowler_matches": "169", "Wicket_taken": "288"}, {"Bowler_name": "AB AGARKAR", "Bowler_matches": "191", "Wicket_taken": "288"}, {"Bowler_name": "Z KHAN", "Bowler_matches": "200", "Wicket_taken": "282"}, {"Bowler_name": "SHAKIB AL HASAN", "Bowler_matches": "215", "Wicket_taken": "277"}, {"Bowler_name": "JH KALLIS", "Bowler_matches": "328", "Wicket_taken": "273"}, {"Bowler_name": "AA DONALD", "Bowler_matches": "164", "Wicket_taken": "272"}, {"Bowler_name": "MASHRAFE MORTAZA", "Bowler_matches": "220", "Wicket_taken": "270"}, {"Bowler_name": "JM ANDERSON", "Bowler_matches": "194", "Wicket_taken": "269"}, {"Bowler_name": "ABDUL RAZZAQ", "Bowler_matches": "265", "Wicket_taken": "269"}, {"Bowler_name": "HARBHAJAN SINGH", "Bowler_matches": "236", "Wicket_taken": "269"}, {"Bowler_name": "M NTINI", "Bowler_matches": "173", "Wicket_taken": "266"}, {"Bowler_name": "N KAPIL DEV", "Bowler_matches": "225", "Wicket_taken": "253"}, {"Bowler_name": "SHOAIB AKHTAR", "Bowler_matches": "163", "Wicket_taken": "247"}, {"Bowler_name": "KD MILLS", "Bowler_matches": "170", "Wicket_taken": "240"}, {"Bowler_name": "MG JOHNSON", "Bowler_matches": "153", "Wicket_taken": "239"}, {"Bowler_name": "HH STREAK", "Bowler_matches": "189", "Wicket_taken": "239"}, {"Bowler_name": "D GOUGH", "Bowler_matches": "159", "Wicket_taken": "235"}, {"Bowler_name": "CA WALSH", "Bowler_matches": "205", "Wicket_taken": "227"}, {"Bowler_name": "CEL AMBROSE", "Bowler_matches": "176", "Wicket_taken": "225"}, {"Bowler_name": "ABDUR RAZZAK", "Bowler_matches": "153", "Wicket_taken": "207"}, {"Bowler_name": "CJ MCDERMOTT", "Bowler_matches": "138", "Wicket_taken": "203"}, {"Bowler_name": "CZ HARRIS", "Bowler_matches": "250", "Wicket_taken": "203"}, {"Bowler_name": "CL CAIRNS", "Bowler_matches": "215", "Wicket_taken": "201"}, {"Bowler_name": "DJ BRAVO", "Bowler_matches": "164", "Wicket_taken": "199"}, {"Bowler_name": "KMDN KULASEKARA", "Bowler_matches": "184", "Wicket_taken": "199"}, {"Bowler_name": "DW STEYN", "Bowler_matches": "125", "Wicket_taken": "196"}, {"Bowler_name": "BKV PRASAD", "Bowler_matches": "161", "Wicket_taken": "196"}, {"Bowler_name": "MA STARC", "Bowler_matches": "99", "Wicket_taken": "195"}, {"Bowler_name": "SR WAUGH", "Bowler_matches": "325", "Wicket_taken": "195"}, {"Bowler_name": "CL HOOPER", "Bowler_matches": "227", "Wicket_taken": "193"}, {"Bowler_name": "L KLUSENER", "Bowler_matches": "171", "Wicket_taken": "192"}, {"Bowler_name": "TG SOUTHEE", "Bowler_matches": "143", "Wicket_taken": "190"}, {"Bowler_name": "M MORKEL", "Bowler_matches": "117", "Wicket_taken": "188"}, {"Bowler_name": "RA JADEJA", "Bowler_matches": "168", "Wicket_taken": "188"}, {"Bowler_name": "CRD FERNANDO", "Bowler_matches": "147", "Wicket_taken": "187"}, {"Bowler_name": "SAEED AJMAL", "Bowler_matches": "113", "Wicket_taken": "184"}, {"Bowler_name": "IMRAN KHAN", "Bowler_matches": "175", "Wicket_taken": "182"}, {"Bowler_name": "AAQIB JAVED", "Bowler_matches": "163", "Wicket_taken": "182"}, {"Bowler_name": "UMAR GUL", "Bowler_matches": "130", "Wicket_taken": "179"}, {"Bowler_name": "SCJ BROAD", "Bowler_matches": "121", "Wicket_taken": "178"}, {"Bowler_name": "NLTC PERERA", "Bowler_matches": "166", "Wicket_taken": "175"}, {"Bowler_name": "NW BRACKEN", "Bowler_matches": "116", "Wicket_taken": "174"}, {"Bowler_name": "IMRAN TAHIR", "Bowler_matches": "107", "Wicket_taken": "173"}, {"Bowler_name": "JDP ORAM", "Bowler_matches": "160", "Wicket_taken": "173"}, {"Bowler_name": "IK PATHAN", "Bowler_matches": "120", "Wicket_taken": "173"}, {"Bowler_name": "A FLINTOFF", "Bowler_matches": "141", "Wicket_taken": "169"}, {"Bowler_name": "TA BOULT", "Bowler_matches": "93", "Wicket_taken": "169"}, {"Bowler_name": "SR WATSON", "Bowler_matches": "190", "Wicket_taken": "168"}, {"Bowler_name": "CH GAYLE", "Bowler_matches": "301", "Wicket_taken": "167"}, {"Bowler_name": "MUSHTAQ AHMED", "Bowler_matches": "144", "Wicket_taken": "161"}, {"Bowler_name": "AU RASHID", "Bowler_matches": "112", "Wicket_taken": "159"}, {"Bowler_name": "RJ HADLEE", "Bowler_matches": "115", "Wicket_taken": "158"}, {"Bowler_name": "SHOAIB MALIK", "Bowler_matches": "287", "Wicket_taken": "158"}, {"Bowler_name": "MD MARSHALL", "Bowler_matches": "136", "Wicket_taken": "157"}, {"Bowler_name": "M PRABHAKAR", "Bowler_matches": "130", "Wicket_taken": "157"}, {"Bowler_name": "A NEHRA", "Bowler_matches": "120", "Wicket_taken": "157"}, {"Bowler_name": "GB HOGG", "Bowler_matches": "123", "Wicket_taken": "156"}, {"Bowler_name": "CR WOAKES", "Bowler_matches": "106", "Wicket_taken": "155"}, {"Bowler_name": "SR TENDULKAR", "Bowler_matches": "463", "Wicket_taken": "154"}, {"Bowler_name": "BAW MENDIS", "Bowler_matches": "87", "Wicket_taken": "152"}, {"Bowler_name": "UDU CHANDANA", "Bowler_matches": "147", "Wicket_taken": "151"}, {"Bowler_name": "R ASHWIN", "Bowler_matches": "111", "Wicket_taken": "150"}]}]   
 
 
# The SQL commands are been used for data definition and data manipulation in cricket data.

# I have created three tables for data definition- PLAYERS, BATSMAN, BOWLER.

# In PLAYERS table there are three attributes(columns)- PLAYERID, NAME AND MATCHES. PLAYERID IS THE PRIMARY KEY WHICH FUNCTIONALLY DEFINES NAME AND MATCHES.


CREATE TABLE PLAYERS
( PLAYERID INT NOT NULL,
 NAME VARCHAR(50) NOT NULL,
 MATCHES INT NOT NULL,
 PRIMARY KEY (PLAYERID ) );
 
# In BATSMAN table there is only two attributes(columns)- PLAYERID AND RUNS. PLAYERID is a foreign key from the table PLAYERS.

CREATE TABLE BATSMAN
( PLAYERID INT NOT NULL,
RUNS INT NOT NULL,
FOREIGN KEY (PLAYERID) REFERENCES PLAYERS(PLAYERID) );

# In BOWLER table there is only two attributes(columns)- PLAYERID AND WICKETS. PLAYERID is a foreign key from the table PLAYERS.

CREATE TABLE BOWLER
( PLAYERID INT NOT NULL,
WICKETS INT NOT NULL,
FOREIGN KEY (PLAYERID) REFERENCES PLAYERS(PLAYERID) );

# BELOW, I have inserted data in the PLAYERS table and all three represent PLAYERID, NAME of cricketer and number of MATCHES he has played in his career.

INSERT INTO PLAYERS VALUES (1,'SR Tendulkar',463);
INSERT INTO PLAYERS VALUES (2,'KC Sangakkara',404);
INSERT INTO PLAYERS VALUES (3,'RT Ponting ',375);
INSERT INTO PLAYERS VALUES (4,'ST Jayasuriya',445);
INSERT INTO PLAYERS VALUES (5,'DPMD Jayawardene',448);
INSERT INTO PLAYERS VALUES (6,'V Kohli',254);
INSERT INTO PLAYERS VALUES (7,'Inzamam-ul-Haq',378);
INSERT INTO PLAYERS VALUES (8,'JH Kallis ',328);
INSERT INTO PLAYERS VALUES (9,'SC Ganguly',311);
INSERT INTO PLAYERS VALUES (10,'R Dravid ',311);
INSERT INTO PLAYERS VALUES (11,'MS Dhoni ',350);
INSERT INTO PLAYERS VALUES (12,'CH Gayle',301);
INSERT INTO PLAYERS VALUES (13,'BC Lara',299);
INSERT INTO PLAYERS VALUES (14,'TM Dilshan',330);
INSERT INTO PLAYERS VALUES (15,'Mohammad Yousuf ',288);
INSERT INTO PLAYERS VALUES (16,'AC Gilchrist ',287);
INSERT INTO PLAYERS VALUES (17,'AB de Villiers',228);
INSERT INTO PLAYERS VALUES (18,'M Azharuddin ',334);
INSERT INTO PLAYERS VALUES (19,'PA de Silva ',308);
INSERT INTO PLAYERS VALUES (20,'RG Sharma',	227);
INSERT INTO PLAYERS VALUES (21,'Saeed Anwar ',247);
INSERT INTO PLAYERS VALUES (22,'S Chanderpaul',268);
INSERT INTO PLAYERS VALUES (23,'Yuvraj Singh ',304);
INSERT INTO PLAYERS VALUES (24,'DL Haynes ',238);
INSERT INTO PLAYERS VALUES (25,'LRPL Taylor',233);
INSERT INTO PLAYERS VALUES (26,'MS Atapattu',268);
INSERT INTO PLAYERS VALUES (27,'ME Waugh ',244);
INSERT INTO PLAYERS VALUES (28,'V Sehwag',	251);
INSERT INTO PLAYERS VALUES (29,'HM Amla',181);
INSERT INTO PLAYERS VALUES (30,'HH Gibbs',248);
INSERT INTO PLAYERS VALUES (31,'Shahid Afridi',398);
INSERT INTO PLAYERS VALUES (32,'SP Fleming',280);
INSERT INTO PLAYERS VALUES (33,'MJ Clarke ',245);
INSERT INTO PLAYERS VALUES (34,'EJG Morgan',246);
INSERT INTO PLAYERS VALUES (35,'Tamim Iqbal',219);
INSERT INTO PLAYERS VALUES (36,'SR Waugh',325);
INSERT INTO PLAYERS VALUES (37,'Shoaib Malik ',287);
INSERT INTO PLAYERS VALUES (38,'A Ranatunga ',269);
INSERT INTO PLAYERS VALUES (39,'Javed Miandad ',233);
INSERT INTO PLAYERS VALUES (40,'Younis Khan',265);
INSERT INTO PLAYERS VALUES (41,'Saleem Malik',283);
INSERT INTO PLAYERS VALUES (42,'NJ Astle',223);
INSERT INTO PLAYERS VALUES (43,'GC Smith',197);
INSERT INTO PLAYERS VALUES (44,'WU Tharanga',235);
INSERT INTO PLAYERS VALUES (45,'MJ Guptill',186);
INSERT INTO PLAYERS VALUES (46,'MG Bevan',	232);
INSERT INTO PLAYERS VALUES (47,'G Kirsten ',185);
INSERT INTO PLAYERS VALUES (48,'A Flower',213);
INSERT INTO PLAYERS VALUES (49,'IVA Richards',187);
INSERT INTO PLAYERS VALUES (50,'BRM Taylor',205);
INSERT INTO PLAYERS VALUES (51,'Mohammad Hafeez ',218);
INSERT INTO PLAYERS VALUES (52,'Shakib Al Hasan',215);
INSERT INTO PLAYERS VALUES (53,'Mushfiqur Rahim ',227);
INSERT INTO PLAYERS VALUES (54,'GW Flower',221);
INSERT INTO PLAYERS VALUES (55,'Ijaz Ahmed ',250);
INSERT INTO PLAYERS VALUES (56,'AR Border ',273);
INSERT INTO PLAYERS VALUES (57,'RB Richardson ',224);
INSERT INTO PLAYERS VALUES (58,'KS Williamson',151);
INSERT INTO PLAYERS VALUES (59,'ML Hayden',	161);
INSERT INTO PLAYERS VALUES (60,'JE Root',152);
INSERT INTO PLAYERS VALUES (61,'S Dhawan',145);
INSERT INTO PLAYERS VALUES (62,'BB McCullum',260);
INSERT INTO PLAYERS VALUES (63,'DM Jones ',164	);
INSERT INTO PLAYERS VALUES (64,'DC Boon',181);
INSERT INTO PLAYERS VALUES (65,'JN Rhodes',245	);
INSERT INTO PLAYERS VALUES (66,'Ramiz Raja ',198);
INSERT INTO PLAYERS VALUES (67,'AD Mathews',	218);
INSERT INTO PLAYERS VALUES (68,'RR Sarwan ',181);
INSERT INTO PLAYERS VALUES (69,'CL Hooper',	227);
INSERT INTO PLAYERS VALUES (70,'SR Watson',169	);
INSERT INTO PLAYERS VALUES (71,'H Masakadza',209);
INSERT INTO PLAYERS VALUES (72,'SK Raina',226);
INSERT INTO PLAYERS VALUES (73,'MN Samuels',	207);
INSERT INTO PLAYERS VALUES (74,'WJ Cronje',188	);
INSERT INTO PLAYERS VALUES (75,'F du Plessis',		143);
INSERT INTO PLAYERS VALUES (76,'DA Warner ',	128);
INSERT INTO PLAYERS VALUES (77,'MEK Hussey ',185);
INSERT INTO PLAYERS VALUES (78,'IR Bell',	161);
INSERT INTO PLAYERS VALUES (79,'A Jadeja ',196);
INSERT INTO PLAYERS VALUES (80,'Q de Kock',124);
INSERT INTO PLAYERS VALUES (81,'DR Martyn',208);
INSERT INTO PLAYERS VALUES (82,'G Gambhir  ',147);
INSERT INTO PLAYERS VALUES (83,'AJ Finch  ',132);
INSERT INTO PLAYERS VALUES (84,'ADR Campbell ',	188);
INSERT INTO PLAYERS VALUES (85,'RS Mahanama',213);
INSERT INTO PLAYERS VALUES (86,'CG Greenidge ',128);
INSERT INTO PLAYERS VALUES (87,'Misbah-ul-Haq',162);
INSERT INTO PLAYERS VALUES (88,'JP Duminy ',199);
INSERT INTO PLAYERS VALUES (89,'PD Collingwood ',197);
INSERT INTO PLAYERS VALUES (90,'A Symonds ',	198);
INSERT INTO PLAYERS VALUES (91,'Abdul Razzaq  ',	265);
INSERT INTO PLAYERS VALUES (92,'M Muralitharan ',350);
INSERT INTO PLAYERS VALUES (93,'Wasim Akram ',	356);
INSERT INTO PLAYERS VALUES (94,'Waqar Younis ',262);
INSERT INTO PLAYERS VALUES (95,'Shahid Afridi',398);
INSERT INTO PLAYERS VALUES (96,'SM Pollock ',303);
INSERT INTO PLAYERS VALUES (97,'GD McGrath ',250);
INSERT INTO PLAYERS VALUES (98,'B Lee',221);
INSERT INTO PLAYERS VALUES (99,'SL Malinga',226);
INSERT INTO PLAYERS VALUES (100,'A Kumble',	271);
INSERT INTO PLAYERS VALUES (101,'WPUJC Vaas ',322);
INSERT INTO PLAYERS VALUES (102,'ST Jayasuriya ',445);
INSERT INTO PLAYERS VALUES (103,'J Srinath  ',229);
INSERT INTO PLAYERS VALUES (104,'DL Vettori',295);
INSERT INTO PLAYERS VALUES (105,'SK Warne ',194	);
INSERT INTO PLAYERS VALUES (106,'Saqlain Mushtaq ',169);
INSERT INTO PLAYERS VALUES (107,'AB Agarkar',191);
INSERT INTO PLAYERS VALUES (108,'Z Khan ',	200);
INSERT INTO PLAYERS VALUES (109,'Shakib Al Hasan',	215);
INSERT INTO PLAYERS VALUES (110,'JH Kallis ',328);
INSERT INTO PLAYERS VALUES (111,'AA Donald ',164);
INSERT INTO PLAYERS VALUES (112,'Mashrafe Mortaza ',220);
INSERT INTO PLAYERS VALUES (113,'JM Anderson',194);
INSERT INTO PLAYERS VALUES (114,'Abdul Razzaq  ',265);
INSERT INTO PLAYERS VALUES (115,'Harbhajan Singh',236);
INSERT INTO PLAYERS VALUES (116,'M Ntini  ',173);
INSERT INTO PLAYERS VALUES (117,'N Kapil Dev ',225);
INSERT INTO PLAYERS VALUES (118,'Shoaib Akhtar',163);
INSERT INTO PLAYERS VALUES (119,'KD Mills',170);
INSERT INTO PLAYERS VALUES (120,'MG Johnson',153);
INSERT INTO PLAYERS VALUES (121,'HH Streak ',189);
INSERT INTO PLAYERS VALUES (122,'D Gough ',159);
INSERT INTO PLAYERS VALUES (123,'CA Walsh ',205);
INSERT INTO PLAYERS VALUES (124,'CEL Ambrose',176);
INSERT INTO PLAYERS VALUES (125,'Abdur Razzak',153);
INSERT INTO PLAYERS VALUES (126,'CJ McDermott ',138);
INSERT INTO PLAYERS VALUES (127,'CZ Harris',250);
INSERT INTO PLAYERS VALUES (128,'CL Cairns ',215);
INSERT INTO PLAYERS VALUES (129,'DJ Bravo',164	);
INSERT INTO PLAYERS VALUES (130,'DW Steyn',125);
INSERT INTO PLAYERS VALUES (131,'BKV Prasad',161);
INSERT INTO PLAYERS VALUES (132,'MA Starc',99);
INSERT INTO PLAYERS VALUES (133,'SR Waugh ',325);
INSERT INTO PLAYERS VALUES (134,'CL Hooper  ',227);
INSERT INTO PLAYERS VALUES (135,'L Klusener ',171);
INSERT INTO PLAYERS VALUES (136,'TG Southee ',143);
INSERT INTO PLAYERS VALUES (137,'M Morkel ',117);
INSERT INTO PLAYERS VALUES (138,'RA Jadeja',168	);
INSERT INTO PLAYERS VALUES (139,'CRD Fernando',147);
INSERT INTO PLAYERS VALUES (140,'Saeed Ajmal',113	);
INSERT INTO PLAYERS VALUES (141,'Imran Khan',175	);
INSERT INTO PLAYERS VALUES (142,'Aaqib Javed ',163);
INSERT INTO PLAYERS VALUES (143,'Umar Gul',130);
INSERT INTO PLAYERS VALUES (144,'SCJ Broad ',121	);
INSERT INTO PLAYERS VALUES (145,'NLTC Perera ',166);
INSERT INTO PLAYERS VALUES (146,'NW Bracken  ',116);
INSERT INTO PLAYERS VALUES (147,'Imran Tahir',107);
INSERT INTO PLAYERS VALUES (148,'JDP Oram',160);
INSERT INTO PLAYERS VALUES (149,'IK Pathan',120);
INSERT INTO PLAYERS VALUES (150,'A Flintoff',141);
INSERT INTO PLAYERS VALUES (151,'TA Boult ',93);
INSERT INTO PLAYERS VALUES (152,'SR Watson ',190	);
INSERT INTO PLAYERS VALUES (153,'CH Gayle',301);
INSERT INTO PLAYERS VALUES (154,'Mushtaq Ahmed ',144);
INSERT INTO PLAYERS VALUES (155,'AU Rashid',112	);
INSERT INTO PLAYERS VALUES (156,'RJ Hadlee ',115);
INSERT INTO PLAYERS VALUES (157,'Shoaib Malik ',287);
INSERT INTO PLAYERS VALUES (158,'MD Marshall',136	);
INSERT INTO PLAYERS VALUES (159,'M Prabhakar ',130	);
INSERT INTO PLAYERS VALUES (160,'A Nehra',120	);
INSERT INTO PLAYERS VALUES (161,'GB Hogg ',123);
INSERT INTO PLAYERS VALUES (162,'CR Woakes ',106);
INSERT INTO PLAYERS VALUES (163,'SR Tendulkar',463	);
INSERT INTO PLAYERS VALUES (164,'BAW Mendis ',87	);
INSERT INTO PLAYERS VALUES (165,'UDU Chandana',147);
INSERT INTO PLAYERS VALUES (166,'R Ashwin ',111);



# Here in BATSMAN table, I have inserted data in two attributes which represent PLAYERID nand number of RUNS he has scored in his career.

INSERT INTO BATSMAN VALUES (1,18426);
INSERT INTO BATSMAN VALUES (2,14234);
INSERT INTO BATSMAN VALUES (3,13704);
INSERT INTO BATSMAN VALUES (4,13430);
INSERT INTO BATSMAN VALUES (5,12650);
INSERT INTO BATSMAN VALUES (6,12169);
INSERT INTO BATSMAN VALUES (7,11739);
INSERT INTO BATSMAN VALUES (8,11579);
INSERT INTO BATSMAN VALUES (9,11363);
INSERT INTO BATSMAN VALUES (10,10889);
INSERT INTO BATSMAN VALUES (11,10773);
INSERT INTO BATSMAN VALUES (12,10480);
INSERT INTO BATSMAN VALUES (13,10405);
INSERT INTO BATSMAN VALUES (14,10290);
INSERT INTO BATSMAN VALUES (15,9720);
INSERT INTO BATSMAN VALUES (16,9619);
INSERT INTO BATSMAN VALUES (17,9577);
INSERT INTO BATSMAN VALUES (18,9378);
INSERT INTO BATSMAN VALUES (19,9284);
INSERT INTO BATSMAN VALUES (20,9205);
INSERT INTO BATSMAN VALUES (21,8824);
INSERT INTO BATSMAN VALUES (22,8778);
INSERT INTO BATSMAN VALUES (23,8701);
INSERT INTO BATSMAN VALUES (24,8648);
INSERT INTO BATSMAN VALUES (25,8581);
INSERT INTO BATSMAN VALUES (26,8529);
INSERT INTO BATSMAN VALUES (27,8500);
INSERT INTO BATSMAN VALUES (28,8273);
INSERT INTO BATSMAN VALUES (29,8113);
INSERT INTO BATSMAN VALUES (30,8094);
INSERT INTO BATSMAN VALUES (31,8064);
INSERT INTO BATSMAN VALUES (32,8037);
INSERT INTO BATSMAN VALUES (33,7981);
INSERT INTO BATSMAN VALUES (34,7701);
INSERT INTO BATSMAN VALUES (35,7666);
INSERT INTO BATSMAN VALUES (36,7569);
INSERT INTO BATSMAN VALUES (37,7534);
INSERT INTO BATSMAN VALUES (38,7456);
INSERT INTO BATSMAN VALUES (39,7381);
INSERT INTO BATSMAN VALUES (40,7249);
INSERT INTO BATSMAN VALUES (41,7170);
INSERT INTO BATSMAN VALUES (42,7090);
INSERT INTO BATSMAN VALUES (43,6989);
INSERT INTO BATSMAN VALUES (44,6951);
INSERT INTO BATSMAN VALUES (45,6927);
INSERT INTO BATSMAN VALUES (46,6912);
INSERT INTO BATSMAN VALUES (47,6798);
INSERT INTO BATSMAN VALUES (48,6786);
INSERT INTO BATSMAN VALUES (49,6712);
INSERT INTO BATSMAN VALUES (50,6684);
INSERT INTO BATSMAN VALUES (51,6614);
INSERT INTO BATSMAN VALUES (52,6600);
INSERT INTO BATSMAN VALUES (53,6581);
INSERT INTO BATSMAN VALUES (54,6571);
INSERT INTO BATSMAN VALUES (55,6564);
INSERT INTO BATSMAN VALUES (56,6524);
INSERT INTO BATSMAN VALUES (57,6248);
INSERT INTO BATSMAN VALUES (58,6173);
INSERT INTO BATSMAN VALUES (59,6133);
INSERT INTO BATSMAN VALUES (60,6109);
INSERT INTO BATSMAN VALUES (61,6105);
INSERT INTO BATSMAN VALUES (62,6083);
INSERT INTO BATSMAN VALUES (63,6068);
INSERT INTO BATSMAN VALUES (64,5964);
INSERT INTO BATSMAN VALUES (65,5935);
INSERT INTO BATSMAN VALUES (66,5841);
INSERT INTO BATSMAN VALUES (67,5835);
INSERT INTO BATSMAN VALUES (68,5804);
INSERT INTO BATSMAN VALUES (69,5761);
INSERT INTO BATSMAN VALUES (70,5757);
INSERT INTO BATSMAN VALUES (71,5658);
INSERT INTO BATSMAN VALUES (72,5615);
INSERT INTO BATSMAN VALUES (73,5606);
INSERT INTO BATSMAN VALUES (74,5565);
INSERT INTO BATSMAN VALUES (75,5507);
INSERT INTO BATSMAN VALUES (76,5455);
INSERT INTO BATSMAN VALUES (77,5442);
INSERT INTO BATSMAN VALUES (78,5416);
INSERT INTO BATSMAN VALUES (79,5359);
INSERT INTO BATSMAN VALUES (80,5355);
INSERT INTO BATSMAN VALUES (81,5346);
INSERT INTO BATSMAN VALUES (82,5238);
INSERT INTO BATSMAN VALUES (83,5232);
INSERT INTO BATSMAN VALUES (84,5185);
INSERT INTO BATSMAN VALUES (85,5162);
INSERT INTO BATSMAN VALUES (86,5134);
INSERT INTO BATSMAN VALUES (87,5122);
INSERT INTO BATSMAN VALUES (88,5117);
INSERT INTO BATSMAN VALUES (89,5092);
INSERT INTO BATSMAN VALUES (90,5088);
INSERT INTO BATSMAN VALUES (91,5080);


# Here in BOWLER table, I have inserted data in two attributes which represent PLAYERID and number of WICKETS he has taken in his career.


INSERT INTO BOWLER VALUES (1,534 );
INSERT INTO BOWLER VALUES (2,502 );
INSERT INTO BOWLER VALUES (3,416 );
INSERT INTO BOWLER VALUES (4,400 );
INSERT INTO BOWLER VALUES (5,395 );
INSERT INTO BOWLER VALUES (6,393 );
INSERT INTO BOWLER VALUES (7,381 );
INSERT INTO BOWLER VALUES (8,380 );
INSERT INTO BOWLER VALUES (9,338 );
INSERT INTO BOWLER VALUES (10,337 );
INSERT INTO BOWLER VALUES (11,323 );
INSERT INTO BOWLER VALUES (12,315 );
INSERT INTO BOWLER VALUES (13,305 );
INSERT INTO BOWLER VALUES (14,293 );
INSERT INTO BOWLER VALUES (15,288 );
INSERT INTO BOWLER VALUES (16,288 );
INSERT INTO BOWLER VALUES (17,282 );
INSERT INTO BOWLER VALUES (18,277 );
INSERT INTO BOWLER VALUES (19,273 );
INSERT INTO BOWLER VALUES (20,272 );
INSERT INTO BOWLER VALUES (21,270 );
INSERT INTO BOWLER VALUES (22,269 );
INSERT INTO BOWLER VALUES (23,269 );
INSERT INTO BOWLER VALUES (24,269 );
INSERT INTO BOWLER VALUES (25,266 );
INSERT INTO BOWLER VALUES (26,253 );
INSERT INTO BOWLER VALUES (27,247 );
INSERT INTO BOWLER VALUES (28,240 );
INSERT INTO BOWLER VALUES (29,239 );
INSERT INTO BOWLER VALUES (30,239 );
INSERT INTO BOWLER VALUES (31,235 );
INSERT INTO BOWLER VALUES (32,227 );
INSERT INTO BOWLER VALUES (33,225 );
INSERT INTO BOWLER VALUES (34,207 );
INSERT INTO BOWLER VALUES (35,203 );
INSERT INTO BOWLER VALUES (36,203 );
INSERT INTO BOWLER VALUES (37,201 );
INSERT INTO BOWLER VALUES (38,199 );
INSERT INTO BOWLER VALUES (39,199 );
INSERT INTO BOWLER VALUES (40,196 );
INSERT INTO BOWLER VALUES (41,196 );
INSERT INTO BOWLER VALUES (42,195 );
INSERT INTO BOWLER VALUES (43,195 );
INSERT INTO BOWLER VALUES (44,193 );
INSERT INTO BOWLER VALUES (45,192 );
INSERT INTO BOWLER VALUES (46,190 );
INSERT INTO BOWLER VALUES (47,188 );
INSERT INTO BOWLER VALUES (48,188);
INSERT INTO BOWLER VALUES (49,187 );
INSERT INTO BOWLER VALUES (50,184 );
INSERT INTO BOWLER VALUES (51,182 );
INSERT INTO BOWLER VALUES (52,182 );
INSERT INTO BOWLER VALUES (53,179 );
INSERT INTO BOWLER VALUES (54,178 );
INSERT INTO BOWLER VALUES (55,175 );
INSERT INTO BOWLER VALUES (56,174 );
INSERT INTO BOWLER VALUES (57,173 );
INSERT INTO BOWLER VALUES (58,173 );
INSERT INTO BOWLER VALUES (59,173 );
INSERT INTO BOWLER VALUES (60,169 );
INSERT INTO BOWLER VALUES (61,169 );
INSERT INTO BOWLER VALUES (62,168 );
INSERT INTO BOWLER VALUES (63,167 );
INSERT INTO BOWLER VALUES (64,161 );
INSERT INTO BOWLER VALUES (65,159 );
INSERT INTO BOWLER VALUES (66,158 );
INSERT INTO BOWLER VALUES (67,158 );
INSERT INTO BOWLER VALUES (68,157 );
INSERT INTO BOWLER VALUES (69,157 );
INSERT INTO BOWLER VALUES (70,157 );
INSERT INTO BOWLER VALUES (71,156 );
INSERT INTO BOWLER VALUES (72,155 );
INSERT INTO BOWLER VALUES (73,154 );
INSERT INTO BOWLER VALUES (74,152 );
INSERT INTO BOWLER VALUES (75,151 );
INSERT INTO BOWLER VALUES (76,150 );
   