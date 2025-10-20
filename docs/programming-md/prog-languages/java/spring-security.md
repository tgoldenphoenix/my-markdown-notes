# Spring Security

**Non-functional Software features**: security, maintainability, performance, scalability, availability. You deal  with  messy code,  memory-related  issues,  multi-threaded design problems, and, of cNurse, security vulnerabilities.

In practice, it’s generally much easier to spot andsolve functional problems than non-functional ones.

User only care about functional requirements (the app is running as expected).

You  can  use  Spring  Security  for  both  standard  web  servlets  and  reac-tive applications. To use it, you need at least Java 8, although the examples inthis book use Java 11, which is the latest long-term supported version.

Like any framework, one of the primary purposes of Spring is to allow you to writeless code to implement the desired functionality. And this is also what Spring Securitydoes.  It  completes Spring as  a  framework  by  helping  you  write  less  code  to  performone  of the most critical aspects of an application—security.

Spring Security provides predefined functionality to help you avoid writing boilerplate code or repeatedly writ-ing the same logic from app to app. But it also allows you to configure any of its com-ponents, thus providing great flexibility.

Security is a complex  subject.  In  the  case  of  a  software  system,  security  doesn’tapply only at the application level. For example, for networking, there are issues to betaken  into  consideration  and  specific  practices  to  be  used,  while  for  storage,  it’sanother  discussion  altogether.  Similarly,  there’s  a  different  philosophy  in  terms  ofdeployment,  and  so  on.  Spring  Security  is  a  framework  that  belongs  to  application-level security (the top-most layer).

Through authentication, an application identifies a user(a  person  or  another  application).  The  purpose  of  identifying  these  is  to  be  able  todecide  afterward  what  they  should  be  allowed  to  do—that’s  authorization.

We classify data as “at rest” or “in transition.” In this context, data at restrefers  to  data  in  computer  storage  or,  in  other  words,  persisted  data. Data  intransition applies to all the data that’s exchanged from one point to another.Different security measures should, therefore, be enforced, depending on thetype of data.

## Vulnerabilities

An excellent start to understanding vulnerabilities is being aware of the Open WebApplication  Security  Project,  also  known  as  OWASP  (<https://www.owasp.org>).  AtOWASP, you’ll find descriptions of the most common vulnerabilities that you shouldavoid  in  your  applications.

Authenticationrepresents  the  process  in  which  an  application  identifies  someone  trying  to  use  it.When someone or something uses the app, we want to find their identity so that fur-ther access is granted or not. In real-world apps, you’ll also find cases in which accessis  anonymous,  but  in  most  cases,  one  can  use  data  or  do  specific  actions  only  whenidentified. Once we have the identity of the user, we can process the authorization.

Authorization is the process of establishing if an authenticated caller has the privi-leges to use specific functionality and data. For example, in a mobile banking applica-tion, most of the authenticated users can transfer money but only from their account.

We can say that we have a broken authorization if a an individual with bad inten-tions  somehow  gains  access  to  functionality  or  data  that  doesn’t  belong  to  them.

**Session fixation** vulnerability is a more specific, high-severity weakness of a web applica-tion. If present, it permits an attacker to impersonate a valid user by reusing a previ-ously generated session ID. This vulnerability can happen if, during the authenticationprocess, the web application does not assign a unique session ID. This can potentiallylead to the reuse of existing session IDs. Exploiting this vulnerability consists of obtain-ing a valid session ID and making the intended victim’s browser use it.

Depending on how you implement your web application, there are various ways anindividual can use this vulnerability. For example, if the application provides the ses-sion ID in the URL, then the victim could be tricked into clicking on a malicious link.If the application uses a hidden attribute, the attacker can fool the victim into using aforeign form and then post the action to the server. If the application stores the valueof the session in a cookie, then the attacker can inject a script and force the victim’sbrowser to execute it.

---

Cross-site scripting, also referred to as XSS, allows the injection of client-side scripts intoweb  services  exposed  by  the  server,  thereby  permitting  other  users  to  run  these.Before being used or even stored, you should properly “sanitize” the request to avoidundesired  executions  of  foreign  scripts.  The  potential  impact  can  relate  to  accountimpersonation  (combined  with  session  fixation)  or  to  participation  in  distributedattacks like DDoS.

---

Cross-site  request  forgery  (CSRF)  vulnerabilities  are  also  common  in  web  applications.CSRF  attacks  assume  that  a  URL  that  calls  an  action  on  a  specific  server  can  beextracted and reused from outside the application (figure 1.8). If the server trusts theexecution without doing any check on the origin of the request, one could execute itfrom any other place. Through CSRF, an attacker can make a user execute undesiredactions on a server by hiding the actions. Usually, with this vulnerability, the attackertargets actions that change data in the system

One of the ways of mitigating this vulnerability is to use tokens to identify the requestor use cross-origin resource sharing (CORS) limitations. In other words, validate theorigin  of  the  request.

---

Be careful of what your server returns to the client, especially, but not limited to, caseswhere  the  application  encounters  exceptions.  

If your appencounters  a NullPointerException  because  the  request  is  wrong  (part  of  it  ismissing,  for  example),  then  the  exception  shouldn’t  appear  in  the  body  of  theresponse.  At  the  same  time,  the  HTTP  status  should  be  400  rather  than  500.  HTTPstatus  codes  of  type  4XX  are  designed  to  represent  problems  on  the  client  side.  Awrong  request  is,  in  the  end,  a  client  issue,  so  the  application  should  represent  itaccordingly. HTTP status codes of type 5XX are designed to inform you that there is aproblem on the server.

Having exception stacks in the response is not a good choice either
