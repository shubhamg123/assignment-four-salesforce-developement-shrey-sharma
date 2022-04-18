# assignment-four-salesforce-developement-shrey-sharma
public class assignment4 {

    public static void q1(list<Account> acclist)
    {

list<opportunity> oplist =new list<opportunity>();
list <Account> acclist1= [select id,name,(Select name from opportunities) from account where id in: trigger.new];
        for(account a:Acclist1)
        {
           
            if(a.opportunities.size()==0)
            {opportunity o =new opportunity();
o.name='first opp '+a.name;
    o.closedate = system.today();
    o.stagename='Prospecting';
    o.accountid=a.id;
               oplist.add(o);
           
           
            }
         
        }
              insert oplist;
       
       
   
   
    }
   
   
     public static void afterupdate(list<Account> acclist)
    {

list<opportunity> oplist =new list<opportunity>();
list <Account> acclist1= [select id,name,(Select name from opportunities) from account where id in: trigger.new];
        for(account a:Acclist1)
        {
           
            if(a.opportunities.size()==0)
            {opportunity o =new opportunity();
o.name='first opp '+a.name;
    o.closedate = system.today();
    o.stagename='Prospecting';
    o.accountid=a.id;
               oplist.add(o);
           
           
            }
         
        }
              insert oplist;
       
       
   
   
    }
   
   

     public static void q2(list<opportunity> oList){
        List<id> Oid=new List<id>();
        List<string> Ost=new List<string>();
        for(opportunity op:oList){
            string s=op.name;
            ost.add(s);
            id i=op.accountid;
            oid.add(i);
        }    
        

        List<opportunity> oplist=[select name,accountid from opportunity where name in :ost and accountid in :oid];
          List<id> Oid2=new List<id>();
        List<string> Ost2=new List<string>();
        for(opportunity o:oplist){
            string s=o.name;
            id i =o.accountid;
            oid2.add(i);
            ost2.add(s);
    }
        for(opportunity o:olist){
            if(oid2.contains(o.accountid)&&ost2.contains(o.name)){
                o.name='duplicate opportunity';
            }
        }}
/*    Public static void Assignment_4_q3(List<account> accList){//AFTER DELETE TRIGGER.OLD//
           List<sobject> bkList=new List<sobject>();
           
         
           For(account ac:accList){
                   if(String.isNotBlank(ac.MasterRecordId))
                   {
                sobject bk= new sobject();
                 bk.Name=ac.Name;
              bk.phone__c=ac.Phone;
                   bkList.add(bk);
                   }
               }
   
           
           insert bkList;
        
        
    }
  */ public static void q4(list<account> oldlist)
 {//Map<Id, aggregateresult> accounttobechecked1= new Map<Id, aggregateresult>([select account.name,count(id) from opportunity where stagename='prospecting' group by account.name]);
    // Map<Id, Account> accounttobechecked= new Map<Id, Account>([select id,(select name from opportunities where stagename='prospecting' ) from account where id in :oldlist]);
    list<account> fdd=[select id,(select name from opportunities where stagename='prospecting' ) from account where id in :oldlist];
     list<account> delerer=new list<account>();
     integer i =0;
     for(account a:oldlist)
     {
      //  integer d=accounttobechecked.get(a.Id).opportunities.size();
        // system.debug(d);
        list<opportunity> f =fdd[i].opportunities;
         if(f.size()>0)
         {
            a.adderror('cannot delete');
         }else
         {
             delerer.add(a);
         }
         i++;
        
 }
     
     
     delete delerer;
     }    Public Static void Assigmnet_4_Q5(List<opportunity> oppList){
       
       
       List<id>accId=new List<id>();
       
       For(opportunity op:oppList){
           if(op.Amount>10000){
               accid.add(op.AccountId);
           }
       }
       
       List<account>accList=[select name,rating,Customerpriority__c from account where id in :accid];
       for(account acc:accList){
           acc.CustomerPriority__c='high';
           acc.Rating='hot';
       }
       update acclist;
       
   }

public static void Q6()
    {
        List<Lead> li = trigger.new;
        for(Lead l:li)
        {
            if(l.LeadSource == 'Phone Inquiry')
            {      
                    database.LeadConvert lc = new database.LeadConvert();
                   
                    lc.setLeadId(l.id);
                   
                    lc.convertedstatus = 'Closed - Converted';
                   
                    database.LeadConvertResult lcr = database.convertLead(lc);
            }
        }
    }



     public static void q7(list<opportunity> bv){
        list<opportunity> tobeupdated=new list<opportunity>();
        for(opportunity t:bv)
        {
            opportunity i=new opportunity();
            i.Name=t.Name;
            i.stagename=t.StageName;
            i.closedate=t.CloseDate;
            i.description=i.Name+'backup';
            tobeupdated.add(i);
        }
        System.debug(tobeupdated);
        insert tobeupdated;
        
        
        
    }
      public static void q8(list<opportunity> newlist,list<opportunity> oldlist,map<id,opportunity> oldmap)
   { 
     list<opportunity> rt=[select id,(select id from tasks) from opportunity where id in:newlist];
     map<id,opportunity> rt1=new map<id,opportunity>([select id,(select id from tasks) from opportunity where id in:newlist]);
       for(opportunity jh:newlist)
       {  list<task> x= rt1.get(jh.id).tasks;
         system.debug(x.size());
           
           if(x.size()>0 && oldmap.get(jh.Id).stagename != 'Closed Won' && oldmap.get(jh.Id).stagename != 'Closed Lost' && jh.stagename != oldmap.get(jh.id).stagename)
           {
              jh.adderror('cannot update it as task are there and stagename was not closed won or lost');
           }else if(x.size()>0 && (oldmap.get(jh.Id).stagename == 'Closed Won' || oldmap.get(jh.Id).stagename == 'Closed Lost'))
           {
               system.debug('though task is therer but it is closed won or lost so can be updated');
               
           }
       else
       {
           
           system.debug('updated');
       }}
   }
    
/*    trigger Assignment_4_Q9 on Opportunity (before insert)
{
    for(Opportunity op:trigger.new)
    {
        AggregateResult rs = [Select count(name)total from opportunity where owner.id =: UserInfo.getUserId()];
        Integer r = Integer.valueOf(rs.get('total'));
        System.debug(r);
       
        if(r>1)
            {
                op.addError('Can\'t Create Opportunity more than 2');
            }
    }
}
*/
     public static void  q10(list<opportunitylineitem> kl,map<id,opportunitylineitem> iu)
   {  map<id,opportunitylineitem> j=new map<id,opportunitylineitem>([select id,unitprice from OpportunityLineItem where unitprice>1000 and id in :kl]);
       list<opportunitylineitem> oo=new list<opportunitylineitem>();
       map<id,product2> po=new map<id,product2>([select id,description from product2]); 
       for(opportunitylineitem yt:kl)
       {
        if(yt.UnitPrice>6000)
        {    string temp= po.get(yt.product2id).description;
           yt.Description='product'+temp+'added successfully';
            
        }else
        {
            yt.adderror('price must be >1000');
        }
           
           
       }
   }
}
  
  
  
  
  
  
  
  
  
  
  triggers
  
  trigger d on Account (before delete) {
  list<account> a=trigger.old;//list of old account
    a1.b2(a);//calling function b2 in class a1
    
}
