# PySWMM - Loop Model - Extract Specific Results 

```python
def ChangeInput(Path,Layer,Object,Attribute,Value): 

    Lines = open(Path).readlines() 
    #SEARCH --there is an issue here, need to ensure S1 doesn't match with S11 
    line = Search(Lines,r'\[' + Layer.upper() + r'\]') #LAYER ROW 
    line2 = Search(Lines[line:],Object) #RELATIVE OBJECT ROW 
    #FIND POSITIONING LIST & HEADER LOCATION 
    Dashes = Lines[line+2][0:-1].split(' ') 
    Header = Lines[line+1] 
    pos = 0 
    for i in range(0,len(Dashes)): 
        Dashes[i] = Dashes[i].replace('-', ' ').replace(';', ' ') 
        if Header[pos:pos+len(Dashes[i])].strip().upper() == Attribute.upper(): 
            break 
        pos = pos + len(Dashes[i]) + 1 
    #REPLACE VALUE 
    Lines[line + line2] = \ 
        Lines[line+line2][0:pos] + \ 
        str(Value) + \ 
        Dashes[i][len(str(Value)):] + \ 
        Lines[line + line2][pos+len(Dashes[i]):] 
    with open(Path, 'w', encoding='utf-8') as file:  
        file.writelines(Lines) 

    #PRINT FOUND ATTRIBUTE NEW VALUE 
    #print(Header[pos:pos+len(Dashes[p])]) 
    #print(Lines[line+line2][pos:pos+len(Dashes[p])].strip()) 

def Search(List,String): 
    RegEx = re.compile(String) 
    for line in range(0,len(List)): 
        if RegEx.search(List[line])!= None: 
            return line 

def SplitTime(String): 
    Date, Time = String.split(' ') 
    Year, Month, Day = Date.split('-') 
    Hour, Minute, Second = Time.split(':') 
    return Year + ',' + Month + ',' + Day + ',' + Hour + ',' + Minute + ',' + Second 

def ChangeOrifice(Path,Value,Orifices): 

    for i in range(0,len(Orifices)): 
        ChangeInput(Path,'ORIFICES',Orifices[i],'Qcoeff',Value) 

from pyswmm import Simulation, Nodes, Links, Subcatchments, SystemStats 
#from pathlib import Path 
import re 

#File Paths 
Path = r'C:\Users\rushwortha\Documents\MyPythonScripts\PCSWMM\Langley\TricklingFilter_v4.inp' 
Output = open(r'C:\Users\rushwortha\Documents\MyPythonScripts\PCSWMM\Langley\Output.csv', 'w') 
#Variable Arrays 
PumpArray = ['TFI-050','TFI-100','TFI-150','TFI-200','TFI-250','TFI-300','TFI-350','TFI-400','TFI-450','TFI-485'] 
OrificeArray = [0.52,0.65,0.78] 
OrificeList = ["C26", "C27", "C28", "C29", "C30", "C31", "C32", "C33", "C34", "C35", "C48", "C49", "C50", "C51", "C52", "C53", "OR1", "OR10", "OR100", "OR101", "OR102", "OR103", "OR104", "OR105", "OR106", "OR107", "OR108", "OR109", "OR11", "OR110", "OR111", "OR112", "OR113", "OR114", "OR115", "OR116", "OR117", "OR119", "OR12", "OR120", "OR121", "OR123", "OR124", "OR125", "OR126", "OR127", "OR128", "OR129", "OR13", "OR130", "OR131", "OR132", "OR133", "OR134", "OR135", "OR136", "OR137", "OR138", "OR139", "OR14", "OR140", "OR141", "OR142", "OR143", "OR144", "OR145", "OR146", "OR147", "OR148", "OR149", "OR15", "OR150", "OR151", "OR152", "OR153", "OR154", "OR155", "OR156", "OR157", "OR158", "OR159", "OR16", "OR160", "OR161", "OR162", "OR163", "OR164", "OR165", "OR166", "OR167", "OR168", "OR169", "OR17", "OR170", "OR171", "OR172", "OR173", "OR174", "OR175", "OR176", "OR177", "OR178", "OR18", "OR19", "OR2", "OR20", "OR21", "OR22", "OR23", "OR24", "OR25", "OR26", "OR27", "OR28", "OR29", "OR3", "OR30", "OR31", "OR32", "OR33", "OR34", "OR35", "OR36", "OR37", "OR38", "OR39", "OR4", "OR40", "OR41", "OR42", "OR43", "OR44", "OR45", "OR46", "OR47", "OR48", "OR49", "OR5", "OR50", "OR51", "OR52", "OR53", "OR54", "OR55", "OR56", "OR57", "OR58", "OR59", "OR6", "OR60", "OR61", "OR62", "OR63", "OR64", "OR65", "OR66", "OR67", "OR68", "OR69", "OR7", "OR70", "OR71", "OR72", "OR73", "OR74", "OR75", "OR76", "OR77", "OR78", "OR79", "OR8", "OR80", "OR81", "OR82", "OR83", "OR84", "OR85", "OR86", "OR87", "OR88", "OR89", "OR9", "OR90", "OR91", "OR92", "OR93", "OR94", "OR95", "OR96", "OR97", "OR98", "OR99"] 

Output.write(\ 
    "Pump, " +  
    "Orifice, " +  
    "SU1_MaxDepth, " +  
    "J13_MaxDepth" +  
    '\n' 
    ) 

for i in range(0,len(PumpArray)): 
    ChangeInput(Path,'PUMPS','P1','Pump Curve',PumpArray[i]) 
    for j in range(0,len(OrificeArray)): 
        ChangeOrifice(Path,OrificeArray[j],OrificeList) 
        with Simulation(Path) as sim: 
            print('Pump:',PumpArray[i], 'Orifice:',OrificeArray[j]) 
            for ind, step in enumerate(sim): 
                next 
            Output.write(\ 
                str(PumpArray[i]) + "," +  
                str(OrificeArray[j]) + "," +  
                str(round(Nodes(sim)["SU1"].statistics['max_depth'],3)) + "," + 
                str(round(Nodes(sim)["J13"].statistics['max_depth'],3)) +  
                '\n' 
                ) 
Output.close() 
```
