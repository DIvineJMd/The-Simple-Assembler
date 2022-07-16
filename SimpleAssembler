import sys
def output(lines):                            
    file=open("stdout.txt","a")
    file.write(lines+"\n")
    return
##############################Stores opcode :address and registers l_i as a binary decimal#######################

instructs={'add':'10000','sub':'10001','movi':'10010','movr':'10011','ld':'10100','st':'10101','mul':'10110','div':'10111','rs':'11000','ls':'11001','xor':'11010','or':'11011','and':'11100','not':'11101','cmp':'11110','jmp':'11111','jlt':'01100','jgt':'01101','je':'01111','hlt':'01010'}
type={'add':'A','sub':'A','movi':'B','movr':'C','ld':'D','st':'D','mul':'A','div':'C','rs':'B','ls':'B','xor':'A','or':'A','and':'A','not':'C','cmp':'C','jmp':'E','jlt':'E','jgt':'E','je':'E','hlt':'F'}
reg={'R0':'000','R1':'001','R2':'010','R3':'011','R4':'100','R5':'101','R6':'110','FLAGS':'111'}
var={}
labels={}
#####################################converts the decimal to binary ####################################
def decimalToBinary(n):                 
    n=int(n)
    val=bin(n).replace("0b", "")
    val=int(val)
    a=''
    while(val>0):         
        x=val%10     
        a+=str(x)
        val=val//10      
    c=8-len(a)
    while(c>0):
        a+='0'  
        c-=1     
    a=a[::-1]
    return a

labels={}

################################store input lines as tuple, line,line2,line3.......\###########################

def lineToBinary(line,ln):
    ans=''
    try:
        if (len(line)<=4):     
            if (line[0]=='mov'):
                if (len(line)==3):
                    if (line[2] in reg): 
                        line[0]='movr'
                    else: 
                        line[0]='movi'
                else:
                    sys.exit()
            try:
                ans+=instructs.get(line[0])
            except SystemExit:
                pass
            except:
                print("Error: line ",ln,"on instruction",line[0])
                sys.exit()
            try:
                typeOfIns=type.get(line[0])
            except SystemExit:
                sys.exit()
            except:
                print("Error: line ",ln,"on type",line[0])
                sys.exit()
            if (typeOfIns=='A'):
                ans+='00'
                ans+=reg.get(line[1])
                ans+=reg.get(line[2])
                ans+=reg.get(line[3])
            if (typeOfIns=='B'):
                ans+=reg.get(line[1])
                st=line[2]
                if ( 0<= int(st[1:]) and int(st[1:])<256):
                    ans+=decimalToBinary(st[1:])
                else:
                    print("Error: in line ",ln,"in (8) bit range",st[1:])
                    sys.exit() 
            if (typeOfIns=='C'):
                ans+='00000'
               
                if(line[0]!='movr' and len(line)>1):
                    assert (line[2]!="FLAGS"), 'Illegal use of FLAGS register'
                    assert (line[1]!="FLAGS"), 'Illegal use of FLAGS register'
                if (line[0]=='movr'):
                    assert (line[2]!='FLAGS'), 'Illegal use of FLAGS register'
                ans+=reg.get(line[1])
                ans+=reg.get(line[2])
            if (typeOfIns=='D'):
                ans+=reg.get(line[1])
                ans+=decimalToBinary(var.get(line[2]))
            if (typeOfIns=='E'):
                ans+='000'
                ans+=decimalToBinary(labels.get(line[1]))
            if (typeOfIns=='F'):
                ans+='00000000000'
        else:
            sys.exit()
    except AssertionError:
        print("illegal use of FLAGS Register in line",ln)
        quit()        
    except SystemExit:
        pass
    except:
        print("Genreal syntax Error: Line is ",ln,"in instruction",line[0],)
        sys.exit()
        
    print(ans)    
    return output(ans)
variabe={}      

##########################################Open/Create STDIN.txt FILE COntains input###########################

file=open("stdin.txt","w")
data=""
address=0
numline=0
timesofvar=0
varerror=0
f=0
countlines=0
afterhlt=0
hltn=0
FLAGS=True

try:
    for data in sys.stdin:
        # print(data)
        if(data!=""):
            if (hltn==0):
                countlines+=1
                if(f==0 and data[0:3]=="var"):
                    timesofvar=timesofvar+1
                        
                else:
                    f=1
                    address+=1
                    if(data[0:3]=="var"):
                        varerror=1
                        numline=countlines
                    l_labels = data.split()
                    if(l_labels[0][-1]==":"):
                    
                        if ((len(l_labels)>1) and (l_labels[1]=='hlt')):
                            hltn+=1
                        labels[l_labels[0][:-1]]=str(countlines-timesofvar-1)
                    if (data[0:3]=="hlt"):
                        # print("1")
                        hltn+=1
                file.write(data)
            else :
                afterhlt+=1
                break
    if (hltn==0):
        print("Error: hlt is not present")
        sys.exit()
      
    if (afterhlt>0):
        print("Error: input taken after hlt")
        sys.exit()
    else:
        if (countlines>256):
            print("memory Exceeded (more than 256)")
            sys.exit()
        la=0

        file=open("stdin.txt","r")
        numberofline=1
        ff=0
        prel=''
    
        for line in file:
            l_i = line.split()
            if(ff==1):
                # print(l_i[1:])
                # print(len(l_i[1:]))
                # print("====================")
                if(l_i[0][-1]==":"):
                    if(len(l_i[1:])==0):
                        print("nested labels not allowed")
                        sys.exit()
                ff=0    
            # print(l_i[0])
            if (l_i[0][-1]!=":"): 
                if (l_i[0]!="var"):
                    lineToBinary(l_i,numberofline)
                    numberofline+=1
                    FLAGS=False
                else:
                    if (len(l_i)==2):
                        if (FLAGS):
                            var[l_i[-1]]=str(address)
                            address=address+1
                            numberofline+=1
                        else:
                            print("Errror: var declared in between")
                            sys.exit()
                    else:
                        print("Error: Var have more than 1 variable")
                        sys.exit()
            else:
                try:
                    if (l_i[0][-1]==":"):
                    
                        if(len(l_i[1:])==0):
                            numberofline+=1
                            prel=((l_i[0][:-1]) )
                            FLAGS=False
                            continue
                        else:
                            l_i=l_i[1:]
                            if (l_i[0]!="var"):
                                lineToBinary(l_i,numberofline)
                            
                                numberofline+=1
                                FLAGS=False
                            else:
                                var[l_i[-1]]=str(address)
                except:
                    print("Error: Line in ",numberofline," on label",l_i[0])
                    sys.exit()
        # print(labels)  
        # print(numberofline)       
        sys.exit()
        
except SystemExit:
    quit()
