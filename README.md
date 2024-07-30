# salesforce-admin-project

Admin and development project 1


trigger newCourseEmail on Course__c (after insert){ list<String> emailid=new list<String>(); list<Candidate_Enquiry__c> address1 =[select Candidate_Name__c, Email__c from Candidate_Enquiry__c]; list<Messaging.SingleEmailMessage> singleEmailMessageList = new List<Messaging.SingleEmailMessage>(); 
for(Candidate_Enquiry__c c:address1){

if(c.Email__c != null){ 
emailid.add(c.Email__c);
}
}
for(Course__c obj: trigger.new){

Messaging.SingleEmailMessage m=new Messaging.SingleEmailMessage(); 
m.setToAddresses(emailid);
m.setSubject(obj.Course_Name__c +' is starting');

m.setPlainTextBody('Dear '+ 'Candidate' +', The following course is starting shortly in our institute.'+ 
' '+
'Course Name: '+ obj.Course_Name__c+
'Course Starting Date :'+obj.Course_Starting_Date__c+
'Course Closing Date: '+obj.Course_Closing_Date__c+
'Course Duration: '+obj.Course_Duration__c+

}

if(singleEmailMessageList != null && singleEmailMessageList.size() > 
0){ Messaging.sendEmail(singleEmailMessageList);
}

}


<apex:page standardController="Student__c" extensions="xyz" recordSetVar="student" tabstyle="Student__c" showHeader="false" sidebar="false">

<apex:sectionHeader title="SWASAKTI" subTitle="Students Attendance 
System"/> <apex:form >
<center>

<apex:outputLabel >Course :</apex:outputLabel> 
<apex:selectList id="one" size="1">
<apex:selectOption itemValue="Cloud Computing" itemLabel="Cloud 

Computing"/> <apex:selectOption itemValue="Core Java" itemLabel="Core 
Java"/> <apex:selectOption itemValue="Tailoring" itemLabel="Tailoring"/> 
<apex:selectOption itemValue="Oracle Financials" itemLabel="Oracle 

Financials"/> <apex:selectOption itemValue="Oracle ADF" itemLabel="Oracle 
ADF"/> <apex:selectOption itemValue="Oracle DBA" itemLabel="Oracle DBA"/> 
<apex:selectOption itemValue="Spoken English" itemLabel="Spoken English"/>

<apex:selectOption itemValue="Communication Skills" itemLabel="Communication Skills"/> 
<apex:selectOption itemValue="Personality Development" itemLabel="Personality 

Development"/> <apex:selectOption itemValue="Yoga" itemLabel="Yoga"/>
<apex:selectOption itemValue="Tally ERP" itemLabel="Tally 
ERP"/> </apex:selectList></center>
<apex:pageBlock > 
<apex:pageBlockSection >

<apex:pageBlockTable id="two" value="{!student}" var="a" columnsWidth="50px,50px" cellpadding="14" border="4">
<apex:column value="{!a.Name}"/> 
<apex:column headerValue="Attandance"> 
<apex:inputCheckbox value="{!a.Name}">

<apex:actionSupport event="onClick" action="{!GetSelected}" rerender="Selected_PBS"/> </apex:inputCheckbox>
</apex:column>
</apex:pageBlockTable>
</apex:pageBlockSection>
<center><apex:commandButton value="Save"/></center>
</apex:pageBlock>
</apex:form> </apex:page>



<apex:page StandardController="Candidate_Enquiry__c" Extensions="redirect_student"> <apex:sectionHeader title="Candidate Enquiry Edit" subtitle="New Candidate 
Enquiry"/> <apex:form >
<apex:outputLink value="{!URLFOR($Action.Candidate_Enquiry__c.New)}"> 
<h4>Create New Candidate</h4>
</apex:outputLink> 
<apex:pageBlock >
<apex:pageBlockSection title="Candidate Information" collapsible="false"> 
<apex:inputField value="{!Candidate_Enquiry__c.Candidate_Name__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Gender__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Guardian__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Date_Of_Birth__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Guardian_Name__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Father_Occupation__c}"/>
<apex:inputField value="{!Candidate_Enquiry__c.Community__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Mother_Occupation__c}"/> <apex:inputField value="{!Candidate_Enquiry__c.Course__c}"/> <apex:inputField value="{!Candidate_Enquiry__c.Family_Annual_Income__c}"/>
<apex:inputField value="{!Candidate_Enquiry__c.Proofs_Submitted__c}"/><br></br> 
<apex:inputField value="{!Candidate_Enquiry__c.Passport_No__c}"/>
</apex:pageBlockSection>
<apex:pageBlockSection title="Educational Details" collapsible="false" columns="1"> 
<apex:inputField value="{!Candidate_Enquiry__c.Educational_Qualification__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Year_of_Pass__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Passed_out_Organization__c}"/>
</apex:pageBlockSection>
<apex:pageBlockSection title="Contact Details" collapsible="false"> 
<apex:inputField value="{!Candidate_Enquiry__c.Mobile_No__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Father_Mobile_No__c}"/>
<apex:inputField value="{!Candidate_Enquiry__c.Address_for_Correspondence__c }"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Email__c}"/>
<apex:inputField value="{!Candidate_Enquiry__c.Permanent_Address__c}"/> 
</apex:pageBlockSection>
<apex:pageBlockSection title="Course Information" collapsible="false"> 
<apex:inputField value="{!Candidate_Enquiry__c.Interested_Course_1__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Other_Interested_Course__c}"/> 
<apex:inputField value="{!Candidate_Enquiry__c.Interested_Course_2__c}"/>
</apex:pageBlockSection>
<center> <apex:commandButton action="{!records}" value="SAVE to student"/> </center>
<center><apex:commandButton action="{!redirect}" value="go" timeout="200"/></center>
</apex:pageBlock>
</apex:form>
</apex:page>
Controller: public class redirect_student
{
String recordId; public redirect_student() {
}
public redirect_student(ApexPages.StandardController controller)
{
//recordId = controller.getId();
} public static void records()
{
saveRecord(); PageReference save;
}
public static void saveRecord()
{
List <Candidate_Enquiry__c> ce =[select id, Address_for_Correspondence__c, Age__c,
Candidate_Name__c, Community__c, Course__c, Date_Of_Birth__c,
Educational_Qualification__c,
Email__c, Employment_Registration_No__c, Employment_Status__c,
Family_Annual_Income__c, Father_Mobile_No__c,
Father_Occupation__c, Gender__c, Guardian__c, Guardian_Name__c, Interested_Course_1__c, Interested_Course_2__c,
Martial_Status__c, Mobile_No__c, Mother_Occupation__c, Other_Interested_Course__c, Passed_out_Organization__c,
Passport_No__c, Permanent_Address__c, Proofs_Submitted__c, Year_of_Pass__c from Candidate_Enquiry__c where id=:ApexPages.currentPage().getParameters().get('ID')];
List<Student__c> stu=new List<Student__c>(); Student__c s;
for(Candidate_Enquiry__c c : ce)
{
s = new Student__c(Name=c.Candidate_Name__c, 
Address_For_Correspondence__c=c.Address_for_Correspondence__c,
Community__c=c.Community__c, Course__c=c.Course__c,
Date_Of_Birth__c=c.Date_Of_Birth__c,
Educational_Qualification__c=c.Educational_Qualification__c, Email__c=c.Email__c,
Family_Annual_Income__c=c.Family_Annual_Income__c, Father_Mobile_No__c=c.Father_Mobile_No__c,
Father_Occupation__c=c.Father_Occupation__c, Gender__c=c.Gender__c, Guardian__c=c.Guardian__c,
Guardian_Name__c=c.Guardian_Name__c,
Interested_Course1__c=c.Interested_Course_1__c, Permanent_Address__c=c.Permanent_Address__c, Year_of_Pass__c=c.Year_of_Pass__c,
Passport_No__c=c.Passport_No__c, Proofs_Submitted__c=c.Proofs_Submitted__c,
Interested_Course_2__c=c.Interested_Course_2__c,
Other_Course_Interested__c=c.Other_Interested_Course__c,
Mobile_No__c=c.Mobile_No__c, Mother_Occupation__c=c.Mother_Occupation__c, Passed_Out_Organisation__c=c.Passed_out_Organization__c ); stu.add(s); database.insert(stu);
delete ce;
}
}
public PageReference redirect()
{ try {
PageReference customPage = new PageReference 
('https://ap1.salesforce.com/a00?fcf=00B90000004OTGB'); customPage.setRedirect(true);
// customPage.getParameters().put('id', recordId); return customPage;
}
catch (DMLException e)
{
ApexPages.addMessage(new ApexPages.message(ApexPages.severity.ERROR, 'Error creating new account.'));
return null;
}
}


