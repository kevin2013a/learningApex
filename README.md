<a href="https://trailblazer.me/id/kevinhorimi" target="_blank" rel="noreferrer"> <img src="https://www.salesforce.com/content/dam/sfdc-docs/www/resources/campaign-assets/live-long-and-propser/images/logo.png" alt="Salesforce" width="150" height="115"/>

# SalesForce Developer


## IF AND ELSE
```cls
// Example of If and Else comparing numbers;

Integer n1 = 10;
Integer n2 = 9;
Integer n3 = 8;

if(n1 > n2 && n1 > n3){
        System.debug('O primeiro número é o maior de todos!');
        } else if (n2 > n1 && n2 > n3){
        System.debug('O segundo número é o maior de todos!');
    } else {
        System.debug('O terceiro número é o maior de todos!');
    }


// Example of If and Else with concatenation and use of conditional AND;
String produto = 'Camiseta';
Integer quantidade;
Decimal preco = 49.99;

if(preco < 30){
    quantidade = 4;
     System.debug('Compra de '+quantidade+' '+produto+'s efetuada com sucesso!');
}else if(preco > 30 && preco <= 50){
    quantidade = 2;
    System.debug('Compra de '+quantidade+' '+produto+'s efetuada com sucesso!');
} else if (preco > 50 && preco <= 80){
    quantidade = 1;
    System.debug('Compra de '+quantidade+' '+produto+' efetuada com sucesso!');
} else {
    System.debug('Ir embora!');
}

```
## ARRAYS
```cls
/* Types of ARRAYS:
SET - Does not accept repeated value (Finds by value. Does not have index);
LIST - Accepts repeated values ​​(It has index and locates by position);
MAP - Stores more information, is in alphabetical order and does not store duplicate keys; */

// Simple example of set;
Set<String> musicStyle = new Set<String>{'Country', 'Rock'};
system.debug(musicStyle);


// Simple example of list;
String [] listaMercado = new String[]{'Batata', 'Cenoura', 'Tomate'};


// Simple example manipulating dates with list;
list<date> datas=new date[5]; 
datas[0] = date.newInstance(2001, 1, 1); 
datas[1] = date.newInstance(2002, 2, 3);
datas[2] = date.newInstance(2003, 3, 3);
datas[3] = date.newInstance(2004, 4, 4);
datas[4] = date.newInstance(2005, 5, 5);

System.debug(datas);

datas.remove(4); 
datas.remove(3);

System.debug(datas);


// Simple map example with string key and decimal return;
Map<String, Decimal>produtos2=new Map<String, Decimal>();
produtos2.put('Prato', 29.90);
produtos2.put('Copo', 9.99);
produtos2.put('Panela', 99.90);

System.debug(produtos2);
System.debug(produtos2.get('Prato')); // Retorna o valor do produto;


// Simple examples of manipulating a list;
list<string> melhoresAlbuns=new String[5];// Definindo o tamanho da lista em 5 posições
melhoresAlbuns.add(0,'Black Ice'); // Adicionando valor a uma posição específica CRIANDO UMA NOVA POSIÇÃO NA LIST
melhoresAlbuns.set(1, 'Acustico MTV'); // Atribui valor a uma posição já existente
melhoresAlbuns[2] = 'Clara Nunes'; // Outra forma de atribuir valor a uma posição já existente

```
## METHODS /  FUNCTIONS
```cls
// Example of a simple function using Static to allow invoking the function without having to instantiate the class;
Public Class Calculadora {
    
    Public Static Void Calculo (Decimal a, String operacao, Decimal b){
        if(operacao == '+'){
            Decimal resultado = a+b;
            System.debug('O resultado é: '+resultado.setScale(2));
        } else if(operacao == '-'){
            Decimal resultado = a-b;
            System.debug('O resultado é: '+resultado.setScale(2));
        } else if(operacao == '/'){
            Decimal resultado = a/b;
            System.debug('O resultado é: '+resultado.setScale(2));
        } else if(operacao == '*'){
            Decimal resultado = a*b;
            System.debug('O resultado é: '+resultado.setScale(2));
        } else{
            System.debug('Operadores inválidos!');
        }
    }
}

// Invoking the function (Without needing to instantiate);
Calculadora.calculo(5,'*',5);
```
## SOQL
```cls
// Simple Query example;
SELECT Id, Name
FROM Account
WHERE Name Like '%Corporation%' // Contém Corporation;


// Example sorting the Query;
SELECT Id, Name
FROM Account
WHERE Name Like 'Dollynho%'
ORDER BY Name DESC NULLS LAST
LIMIT 10


// Example grouping the Query by name;
SELECT Name, Count(Id)
FROM Opportunity
GROUP BY Name
HAVING Count(Id)>1


// SubQuery example:
SELECT Id, Name,
(SELECT Id, Name, Amount
FROM Opportunities)
FROM Account 


// Using several SubQuery:
SELECT Id, Name,
( SELECT Id, Name
FROM Contacts),
( SELECT Id, Name
FROM Opportunities),
( SELECT Id, Name
FROM Despesas__r),
( SELECT Id, Name
FROM Projetos__r)
FROM Account
```
## APEX
```cls
// Delete all records that were created between July 1st and August 2nd by the Query;
List<Projeto__c> lstProjeto = [SELECT Id, numeroProjeto__c, CreatedDate FROM Projeto__c WHERE CreatedDate>=2022-07-01T00:00:00Z AND CreatedDate<2022-08-03T00:00:00Z];

for(Projeto__c Count: lstProjeto){
    DELETE Count;
}


// Delete all records that were created between July 1st and August 2nd with IF;
List<Projeto__c> lstProjeto = [SELECT Id, numeroProjeto__c, CreatedDate FROM Projeto__c];
for(Projeto__c Count: lstProjeto){
    Date DataInicio = Date.newInstance(2022, 07, 01);
    Date DataFim = Date.newInstance(2022, 08, 03);
    if(Count.CreatedDate>= DataInicio && Count.CreatedDate<DataFim){
        DELETE Count;
    }
}


// Simple example creating record with more than one RecorType via Apex;
Despesa__c newDespesa = new Despesa__c();
    newDespesa.RecordTypeId = '0128Z000000dDp8QAE';// Atribuindo um tipo de registro pelo Id do tipo de registro
    insert newDespesa;


// Simple example inserting several records with loop;
List<Lead> lstLead = new List<Lead>(); // Criando uam lista;

for(Integer i = 1; i < = 5; i++){ 
    Lead newLead = new Lead();
    String numero = String.valueOf(i);
    newLead.LastName = 'Marcos '+ numero;
    newLead.Company = 'Marcos Company'+ numero;
    newLead.Status = 'Open - Not Contacted';
    lstLead.add(newLead); // Adicionando os registros na lista;
}
 INSERT lstLead; // Salva a lista toda de uma vez;
 
 
// Example updating all records without despesas to 'Sem Descrição':
List<Despesa__c> lstDesp = [SELECT Id, Name, TipoDespesa__c FROM Despesa__c];

for(Despesa__c Counter: lstDesp){
    if(Counter.TipoDespesa__c==null){
        Counter.TipoDespesa__c = 'Sem Descrição';
        UPDATE Counter;
    }
}
```
## TRIGGER
```cls
// Simple example of Trigger structure organized by time and action;
Trigger LeadTrigger on Lead (before INSERT, before UPDATE, after INSERT, after UPDATE){
    
    if(Trigger.isBefore && Trigger.isInsert){
        LeadBO.tamanhoEmpresa(Trigger.new);
        
    }else if(Trigger.isBefore && Trigger.isUpdate){
        LeadBO.tamanhoEmpresa(Trigger.new);
        
    }else if(Trigger.isAfter && Trigger.isInsert){
        LeadBO.verificarCadastro(trigger.new);
        
    }else if(Trigger.isAfter && Trigger.isUpdate){
   
    }
}
```
## BO / HANDLER
```cls
// Simple example of a BO class that houses the methods called by the Trigger;
Public Class LeadBO {
    
    // Creates a task whenever a Lead with AnnualRevenue over 500,000.00 is created;
    Public Static Void verificarCadastro (List<Lead> lstLead){
        List<Task> lstTask = new List<Task>();
        
        for(Lead Counter: lstLead){
            if(Counter.AnnualRevenue > 500000){
                task newTask = new Task();
                newTask.Subject = 'Verificar se o cadastro está completo';
                newTask.Status = 'Not Started';
                newTask.Priority = 'Normal';
                newTask.WhoId = Counter.Id;
                lstTask.add(newTask); 
            }
            if(lstLead != NULL){
            INSERT lstTask;
            }
        }    
    }
    
    
   // Assign the company size as Small when the number of employees is less than 1000;
    Public Static Void tamanhoEmpresa(list<Lead> lstLead){ 
        for(Lead Counter2: lstLead){
            if(Counter2.NumberOfEmployees<1000){
                Counter2.TamanhoEmpresa__c = 'Pequena';
            }
        }  
    } 
}
```
## TEST CLASS
```cls
// Simple test class example: testing all calculator possibilities;
@IsTest
Public Class CalculadoraTest {
    
    @IsTest
    Public Static Decimal testarMetodoSoma(){
        Decimal resultSoma = Calculadora.Calculo(10,'+',5);
        System.assert(resultSoma==15, 'Resultado da soma não é o esperado!');
        return resultSoma;
    }

    @IsTest
    Public Static Decimal testarMetodoSubtracao(){
        Decimal resultSubtracao = Calculadora.Calculo(10,'-',5);
        System.assert(resultSubtracao==5, 'Resultado da subtração não é o esperado!');
        return resultSubtracao;
    }
    
    @IsTest
    Public Static Decimal testarMetodoDivisao(){
        Decimal resultDivisao = Calculadora.Calculo(10,'/',5);
        System.assert(resultDivisao==2, 'Resultado da divisão não é o esperado!');
        return resultDivisao;
    }
    
    @IsTest
    Public Static Decimal testarMetodoMultiplicacao(){
        Decimal resultMultiplicacao = Calculadora.Calculo(10,'*',5);
        System.assert(resultMultiplicacao==50, 'Resultado da divisão não é o esperado!');
        return resultMultiplicacao;
    }
    
    @IsTest
    Public Static Decimal testarError(){
        Decimal error = Calculadora.Calculo(10,'&',5);
        System.assert(error==0, 'Resultado da divisão não é o esperado!');
        return error;
    }
}


// Simple test class example for records;
@IsTest
Public Class CaseTest {

    // Testing the validation when inserting the record
    @IsTest
    public static void ifIsCreated(){
        Case newCase   = new Case(); 
        newCase.Status = 'New';
        newCase.Origin = 'Web';
        INSERT newCase;
        Case casoCriado = [SELECT Id, Type FROM Case WHERE Id=:newCase.id];
        System.assert (newCase.Type == 'Other', 'O caso não esta igual a Other');
    }
    
    // Testing the validation when updating the record;
    @IsTest
    public static void ifIsUpdate(){
        
        Case newCase   = new Case(); 
        newCase.Status = 'New';
        newCase.Origin = 'Web';
        newCase.Type = 'Electronic';
        INSERT newCase;
        newCase.Type = NULL;
        UPDATE newCase;
        System.assert (newCase.Type == 'Other', 'O caso não esta igual a Other');
    }
}
```
