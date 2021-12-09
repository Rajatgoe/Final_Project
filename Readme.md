# FINAL PROJECT CRICKET WEB SCRAPING 


![image](Images/206351.png)


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


######
# The SQL commands are been used for data definition and data manipulation in cricket data.

# I have created three tables for data definition- PLAYERS, BATSMAN, BOWLER.

# In PLAYERS table there are three attributes(columns)- PLAYERID, NAME AND MATCHES. PLAYERID IS THE PRIMARY KEY WHICH FUNCTIONALLY DEFINES NAME AND MATCHES.


CREATE TABLE PLAYERS
( PLAYERID INT NOT NULL,
 NAME VARCHAR(50) NOT NULL,
 MATCHES INT NOT NULL,
 PRIMARY KEY (PLAYERID ) );
 
# In BATSMAN table there is only two attributes(columns)- PLAYERID AND RUNS.

CREATE TABLE BATSMAN
( PLAYERID INT NOT NULL,
RUNS INT NOT NULL);

# In BOWLER table there is only two attributes(columns)- PLAYERID AND WICKETS. .

CREATE TABLE BOWLER
( PLAYERID INT NOT NULL,
WICKETS INT NOT NULL);


### Above, I have created the Attributes(columns) in the pymysql. This code is been written to 
### insert the rows details in to the column.The json output from the above code is 
### been used as an indexing and inserting row detail of the players name, matches been played, runs  ### been scored in the careed and total wicket been taken in the career.

### First I have imported the library pymysql  to work with mysql ,connected it to mysql address and created a connection with pymysql.
import pymysql

conn = pymysql.connect(host='mysql.clarksonmsda.org', port=3306, user='ia626',
                       passwd='ia626clarkson', db='ia626', autocommit=True)

cur = conn.cursor(pymysql.cursors.DictCursor)

### Json file has list of dictionaries, from list(cricket) first dictionary has been taken, which I indexed as [0] then from list["batsman_detail"] been selected(Details about all the batsman).
# For inserting the batsman detail in the column, first we have to check about each unique batsman name in the players table(A bowler could be a batsman too), for that we writter a query (sql = 'SELECT * from PLAYERS where NAME =  %s') %s refers to (batsman['Batsman_name']) and in the next line we have executed(cur.execute(sql,(batsman['Batsman_name']))) the same sql query.
# Next step, we went inside the cur and if playerid(it is auto increment in pymysql) is none, i have added (batsman name and batsman matches) using above line method. 
### (playerid = cur.lastrowid) it means till last row.
### last step of this loop, is to connect Auto Increment (playerid) of player table and playerid in batsman table and insert the runs scored in batsman table.
for batsman in cricket[0]["batsman_detail"]:
    sql = 'SELECT * from PLAYERS where NAME =  %s'
    cur.execute(sql,(batsman['Batsman_name']))
    playerid = None
    for row in cur:
        playerid = row['PLAYERID']
    if playerid is None:
        sql = 'INSERT INTO PLAYERS (`NAME`,`MATCHES`) VALUES (%s,%s)';
        cur.execute(sql,(batsman['Batsman_name'],batsman['Batsman_matches']))
        playerid = cur.lastrowid
    sql = 'INSERT INTO BATSMAN VALUES (%s,%s)';
    cur.execute(sql,(playerid,batsman['Runs_scored']))
    
###  SAME PROCESS FOR BOWLERS TABLE   
### Json file has list of dictionaries, from list(cricket) second dictionary has been taken, which I indexed as [1] then from list["bowler_detail"] been selected(Details about all the bowler).
# For inserting the bowler detail in the column, first we have to check about each unique bowler name in the players table(A batsman can be a bowler too), for that we writter a query (sql = 'SELECT * from PLAYERS where NAME =  %s') %s refers to (bowler['Bowler_name'])) and in the next line we have executed(cur.execute(sql,(bowler['Bowler_name']))) the same sql query.
# Next step, we went inside the cur and if playerid(it is auto increment in pymysql) is none, i have added (bowler name and bowler matches) using above line method. 
### (playerid = cur.lastrowid) it means till last row.
### last step of this loop, is to connect Auti increment(playerid) of player table and (playerid) in bowlers table and insert the Wicket_taken in bowler table.   
    
for bowler in cricket[1]["bowler_detail"]:
    sql = 'SELECT * from PLAYERS where NAME =  %s'
    cur.execute(sql,(bowler['Bowler_name']))
    playerid = None
    for row in cur:
        playerid = row['PLAYERID']
    if playerid is None:
        sql = 'INSERT INTO PLAYERS (`NAME`,`MATCHES`) VALUES (%s,%s)';
        cur.execute(sql,(bowler['Bowler_name'],bowler['Bowler_matches']))
        playerid = cur.lastrowid
    sql = 'INSERT INTO BOWLER VALUES (%s,%s)';
    cur.execute(sql,(playerid,bowler['Wicket_taken']))
        
    
 


## Query to get top 10 bowlers showing wickets and  name

SELECT BO.WICKETS, P.NAME
FROM BOWLER BO,PLAYERS P
WHERE BO.PLAYERID = P.PLAYERID AND
BO.WICKETS >= 337

## Query to get top 10 batsman showing runs and name
SELECT BA.RUNS, P.NAME
FROM BATSMAN BA,PLAYERS P
WHERE BA.PLAYERID = P.PLAYERID AND
BA.RUNS >= 10889

![image](Images/246097.png)
![image](Images/273050.png)


