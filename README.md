# Deployment Manager - Full Production 

* pip env
```
sudo apt-get install virtualenv
virtualenv -p python3 venv
source venv/bin/activate
```
* show put forward ip
```
gcloud compute forwarding-rules list
```

* set default compute zone 
```
gcloud config set compute/zone us-central1-f

```

```
gcloud source repos list
```

--- 

Base:
```
for i in {1..10}; do
  echo Hello, World!
done 
```

Powershell:
```
for($i=1; $i -le 10; $i++){
  Write-Host "Hello, World!"
}
```

# Python String Method 
```
stringvar.index('letter')
stringvar.upper()
stringvar.lower()
stringvar.strip()
string.rstrip()
string.lstrip()
string.count('letter')
string.endswith('word')
string.isnumeric()

int('123')
" ".join(['this','is','string')
"this is string".split()

"{} is {}".format(var1,var2)
"{var2} is {var1}".format(var1=var1,var2=var2)

"{:.2f}".format(var) # 2 float 
"{:>3} F | {:>6.2f} C".format(x,y) # format to the right 
```

```
substring in string
```


|Expr	|Meaning|	Example|
|---|---|---|
|{:d}	|integer value|	'{:d}'.format(10.5) → '10'|
|{:.2f}	|floating point with that many decimals	|'{:.2f}'.format(0.5) → '0.50'|
|{:.2s}|	string with that many characters	|'{:.2s}'.format('Python') → 'Py'|
|{:<6s}|	string aligned to the left that many spaces|	'{:<6s}'.format('Py') → 'Py    '|
|{:>6s}|	string aligned to the right that many spaces|	'{:>6s}'.format('Py') → '    Py'|
|{:^6s}	|string centered in that many spaces |	'{:^6s}'.format('Py') → '  Py '|
