## LECTURE 1: Intro to the course

Attributes of a database system:
* Massive - handle huge amounts of data
* Persistent - data outlives programs that execute on data
* Safe - data will always be in a consistent state inspite of power, network utages, etc.
* Multiuser - data can be accessed concurrently by different programs
* Convenient - makes it easy to work with large amounts of data. There's a notion in databases called 'physical data independence,' where programs are completely agnostic to how data is actually stored. E.g. Query languages are declarative, as they allow you to access data without specifying algorithm.
* Efficient - three most important features of databases 1. perf 2. perf 3. perf
* Reliable - Critically important that 99.999% of the time database should  be available

Aspects of a database system:
* Database applications may be programmed via frameworks, e.g. django, ruby on rails, etc.
* Database applications may not necessaril use DBMS e.g. excel spreadsheets. Hadoop is a data processing system that works on files.
* Applications servers, web servers are middleware that allow apps to access data.

### Key concepts
* Data model
** set of records
** XML
** graph
* Schema - structure of a database is defined using a data definition language)
Schema is set at the beginning and does not change.
* Data manipulation language (DML) - for querying a language

### People involved with databases
* DBMS implementer - builds system
* Database designer - establishes schema
* Database application developer - writes programs that uses database
* Database administrator - loads data, keeps it running smoothly. Databases have tuning parameters that need to be set properly for efficient operations.

## LECTURE 2 - The Relational Model

* Used by all major commercial database systems - spawned a billion $ implementation
* simple model that can be implemented efficiently

+ Database = set of named relations( or tables)
+ Each relation has a set of named attributes (or columns)
+ Each tuple (or row) has a value for each attribute
+ Each attribute (or column) has a type. Many databases also support structure types for attributes.
+ NULL  - special value for "unknown" or "undefined". We have to be careful about using NULL values in databases. For e.g. gpa > 3.5 OR gpa <= 3.5 will return all records except the one with gpa = NULL.
+ Key - attribute whose value is unique in each tuple. Or set of attributes whose combined values are unique.

create Table Student(ID, name, GPA, photo) - to create a table in SQL
create Table College
   (name string, state char(2), enrollment integer) - to specify types for attributes

LECTURE 3 - Querying Relational Databases
=========================================

+ Database people have the habit of representing databases as gigantic disks

Steps in using a database:
1. Design schema; create using DDL
2. "Bulk load" initial data
3. Repeat: execute queries and modifications

A relational database allows "ad-hoc queries in high-level language"
* Some queries are easy to pose and hard to execute and vice versa
* Query language is a subset of DML, which also allows modifications

Compositionality - ability to run a query on the result of another query.
Closed language - when the result of a query is the same type of object as a query

Query languages
```````````````
* Relation algebra - formal
   + pi (result of query) sigma (predicate) infinity symbol (joins tables)
* SQL - actual/implemented
   + select (result of query) from (what tables to run query on) where (predicate)

LECTURE 4: XML Data - Introduction, Well-formed XML
===================================================

* XML (Extensible markup language) is an alternate model to relational model.
* Standard for data representation and exchange. Designed for the web.
* XML tags represent structure of data rather than formatting.

* Element(open-close tags and data), attributes (element's properties)

Difference between XML and database?
+ Structure - labels v. hierarchical (tree) or graph
+ Schema is fixed in advance for database, XML is self-describing and flexible.
+ Simple nice languages for querying databases, Less so for XML.
+ No ordering in database, ordering is implied in XML
+ Native implementation on many systems for DBs, XML is just an add on and may be based on a relational DB.

Well-Formed XML: most flexible XML. If documents adhere to XML standard, it's well-formed
+ single root element
+ matched tags with proper nesting
+ unique attributes within elements
XML document -> XML parser (tells you if XML is well formed) -> parsed XML

Displaying XML: Use a rule-based language to translate to HTML e.g.CSS, XSL
XML document -> CSS/XSL interpreter (rules are specified here) -> HTML document

LECTURE 5: XML Data: DTDs ( Document Type Descriptors), IDs and IDREFs
======================================================================

"Valid" XML : besides being well-formed, the document also adheres to basic structural requirements. Common ways of specifying XML structure are
+ Document Type Descriptor (DTD)
+ XML Schema (XSD)

XML document -> Validating parser ( DTD or XSD input) -> Parsed XML

DTD:
* Grammar-like language for specifying elements, nesting, attributes, ordering, #occurences
* Also special attribute types ID and IDREFs which allow you to specify pointers within document, although these pointers are untyped.

Benefits of DTD/XSD :
1. Programs can be simpler as they can assume a structure
2. CSS/XSL can assume a strcuture
3. It's a specification for XML.
4. Documentation on what data actually looks like. Offers the benefit of "typing."

Disadvantages :
1. Flexibility of XMLs is lost because of DTDs
2. DTDs can be really messy for irregular documents

DTD is specified before file in the same document. In a DTD, all attribute values are treated as strings.

E.g. of DTD

<!DOCTYPE Bookstore [
   <!ELEMENT Bookstore (Book | Magazine)*>
-- says the XML can contain a single Bookstore root element, which can contain any number of Book or Magazine elements.

   <!ELEMENT Book (Title, Authors, Remark?)>
-- a Book element can contain Title, Authors and Remark elements only exactly in that order. ? says that Remark is optional.

   <!ATTLIST Book ISBN CDATA #REQUIRED
                  Price CDATA #REQUIRED
                  Edition CDATA #IMPLIED>

CDATA -- just string
ISBN and Price have to be present
Edition may not be present

   <!ELEMENT Title (#PCDATA)>
#PCDATA means Title is a string. It can be an empty string.

   <!ELEMENT Authors (Author+)>
means Authors can have one or more Author elements

   <!ELEMENT Author(First_Name, Last_Name)>
-- Author element consists of First_Name and Last_Name elements in that order.
   <!ELEMENT First_Name (#PCDATA)>
   <!ELEMENT Last_Name (#PCDATA)>
]>

xmllint can be used to check for XML validity.

<Bookstore>
   <Book ISBN="12321-1231" Price="100" Authors="JU SK">
      <Title>First course</Title>
   </Book>            
   <Author Ident="JU">
      <First_Name>Jeffrey</First_Name>
      <Last_Name>Ullman</Last_Name>
   </Author>
   <Author Ident="SK">
      <First_Name>Sunil</First_Name>
      <Last_Name>Kowlgi</Last_Name>
   </Author>

</Bookstore>

Ident is an ID attribute. It needs to have a unique value.
Authors is an IDREFs attribute. It's value can point to one or more elements and is typeless.

IDREF - single attribute value
IDREFS - one or more attribute values

DTDs cannot have typed pointers, but XML schema can.

LECTURE 6: XML DATA - XML SCHEMA
================================

XSD (XML Schema)
1.Similar to DTDs
2.Also data types, keys, (typed) pointers and more.

XSD is usually in a separate file from the XML.
XSD is written in XML, except it has special tags.

LECTURE 7: RELATIONAL ALGEBRA (1)
===================================================

Relational algebra forms the underpinnings of an implemented language like SQL.

Result of the query is a new relation.

1. Simplest query in relational algebra: Student
Returns a copy of the Student relation

2. Select operator: used out pick certain rows out of a relation
E.g. Student with GPA > 3.7
sigma ( selection operator)

3. Project operator: picks certain columns
pi ( project operator)

pi ( <sigma condition> <Relation> ) is how we pick certain rows and columns

4. Cross-product: takes two relations of m and n tuples each and generates a relation with m*n tuples, without discarding duplicates.

5. Natural join: performs a cross-product but
* Enforces equality on shared attributes.
* Eliminates one copy of duplicate attributes.
Essentially, it is a combination of tuples that are equal in their common attribute names.
Written using a bowtie symbol. It is purely for expressing conveniently, and everything you can do with natural join can be achieved with cross-product.

6. Theta Join - it's an abbreviation and does not add expressiveness to the language. It's a basic operation in DBMS languages. When people use the word join, they generally mean 'theta join.' It basically applies a condition to the cross product of two relations.

LECTURE 8: RELATIONAL ALGEBRA (2) - SET OPERATORS, RENAMING, NOTATION
===================================================================

1. Union operators
natural join and cross product operations result in matching tuples from different relations with one another. The union operator vertically joins various tuples.

2. Difference operator
Eliminate tuples from one relation where the key also exists in another relation.

3. Intersection
Does not add expressiveness to the language. Returns tuples that are the same between relations.

4. Rename
Uses Greek symbol rho.
* Used to unify schemas for set operators.
* One of the most important uses is for disambiguations in "self-joins".
E.g. Find pairs of colleges in same state.

Alternate notation
Trees and linear representations.

LECTURE 9: INTRODUCTION TO SQL
=============================

* Pronounced as  sequel.
* Supported by all commercial database systems
* thousands of pages in the SQL standards
* Most common use is to embed sql in programs
* Declarative language -- simple queries that specify what you want, and not how you want it. Query optimizer takes SQL query and figures out how ot optimize execution.

DDL
- create table
- drop table
DML
- select for querying
- insert
- delete
- update

Select is the bread and butter of SQL. Consists of 3 clauses - select, from, where.

select A1, A2, A3
from R1, R2, R3
where cond1, cond2

equivalent relational algebra notation:

project over A1, A2, A3 (select cond1, cond2 from R1xR2xR3)
basically, project over these features, where the tuples from the cross product of R1, R2 and R3 satisfy cond1, cond2.

LECTURE 10: SQL - basic Select statement
========================================
SQL we have duplicates unlike relational algebra as it uses multiset as opposed to a set.

Use the distinct keyword to eliminate duplicates.

SQL is an unordered model. If you want an ordered result, you have to specify the 'order by' clause.

like - regular expression matching in SQL query conditions.
You can also have arithmetic in arguments to select.

LECTURE 11: Table Variables and Set operators
=============================================

Table variables are the ones used in the 'from' clause.

You can
* rename relation
* use union, intercept and except operations

Helpful to rename relation if you want to run cross product on same table.

Union operator in SQL by default eliminates duplicates, so it has to internally sort results. Therefore the results always show up as sorted, unless you use the 'union all' clause.

e.g.

select cName as name from College
union all   // does not elim. duplicates
select sName as name from Student
order by name // sort in ascending order by default

Intersect operator
------------------
select sID from Apply where major = 'CS'
intersect
select sID from Apply where major = 'EE'

Some databases don't support intersect operator, but they don't lose any expressive power.

Another way to find intersection :
select distinct A1.sID
from Apply A1, Apply A2
where A1.sID = A2.sID and A1.major = 'CS' and A2.major = 'EE'

'except' is the difference operator. There are some queries that cannot be written without this operator.

E.g.

select sID from Apply where major  ='CS'
except
select sID from Apply where major = 'EE;

The above expression will return values of students who only applied to CS and not EE.

The above expression cannot possibly be reproduced without the except operation. For instance

select sID
from Apply A1, Apply A2
where A1.sID = A2.sID and major = 'CS' and major <> 'EE';

This expression is not equivalent to the one using 'except' as it returns all students who major in CS and also have a non-EE major, which includes CS! Therefore any student that majors in CS will automatically appear in the result.

LECTURE 12: Subqueries WHERE clause
===================================

To calculate GPA

Select GPA
From  Student
Where sID in (select sID from Apply where major = 'CS')
- Here you get the exact list of unique CS student GPAs

select GPA
from Student, Apply
where Student.sID = Apply.sID and major = 'CS'
- Here you will get GPA duplicates because you're joining the Student and Apply relations. Also, you don't want to avoid duplicate GPAs cause two students may have the same GPA. Therefore using a subquery is the only correct way to write this query.

With subqueries you can write expressions without the except operator. Taking the previous example,

select sID, sName
from Student
where sID in (select sID from Apply where major = 'CS')
and sID not in (select sID from Apply where major = 'EE');

in or not in - to check membership
exists - to check if a relation is empty or not

Correlated reference - when a subsquery uses a relation that comes from the enclosing query.

select cName, state
from College C1
where exists (select * from College C2
              where C2.state = C1.state and
              C1.cName <> C2.cName)

C1 is a correlated reference in the above query.

How to find a max/min value?
E.g.
select sName, GPA
from Student C1
where not exists (select * from Student C2
                  where C2.GPA > C1.GPA);

Just by using joins it's not possible to find a max value. You have to use a subquery.

'all' keyword - check if a value has a certain relationship with all elements of subquery.

E.g.
select sName, GPA
from Student S1
where GPA > all (select GPA from Student S2);

The above query returns the sName of the highest GPA student.

'any' keyword - check if a value is sataisfied with one or more elements of subquery.

Any and all are very convenient, but they do not give expressive power. The same can be achieved using 'exists and not exists.

LECTURE 13: Subqueries FROM and SELECT clauses
==============================================

Subqueries can also be used in select and from clauses.    

When you use a subquery in a select clause, that subquery should return exactly one result.

LECTURE 14: Aggregation
=======================

Select A1, A2, A3 ( aggregation functions over values in multiple rows e.g. min, max, sum, avg)
From
Where (applies to single rows at a time)
Group By columns ( allows us to partition results into groups)
Having condition (applies to groups, allows us to have filters)

Simplest aggregation query:
select avg(GPA)
from Student;

There's a problem with this query:
select avg(GPA)
from Student, Apply
where Student.sID = Apply.sID and major = 'CS';

Some students would have applied to CS in many colleges so we may double-count certain students' GPA.

To fix the problem, we do this --
select avg(GPA)
from Student
where sID in (select sID from Apply where major = 'CS');

count function - returns the number of tuples in the result.

E.g.
We'll attempt to count the number of applicants to Cornell
select count(*)
from Apply
where cName = "Cornell"

But, there's a problem with the above query. It will return the number of applications to Cornell, and there could be multiple applications per applicant.

SQL provides a simple way of fixing the above query:
select count(distinct sID)
from Apply
where cName = "Cornell";

A slightly contrived example, to select the students such that number of other students with the same GPA is equal to the number of other students with the same sizeHS.

select *
from Student S1
where (select count(*) from Student S2
       where S2.sID <> S1.sID and S2.GPA = S1.GPA) =
      (select count(*) from Student S2
       where S2.sID <> S1.sID and S2.sizeHS = S1.sizeHS);

Group by is only used in conjunction with aggregation.
E.g.
To count the number of applicants to each college
select cName, count(*)
from Apply
group by cName;

Grouping takes a relation and partitions it by values of a certain attribute.

Total enrollment of college students for each state:
select state, sum(enrollment)
from College
group by state;

A more complicated query:
select cName, major, min(GPA), max(GPA)
from Student, Apply
where Student.sID=Apply.sID
group cName, major

The above query finds the min and max GPAs of applicants to each college and major.

The having clause applies a filter to the grouping clause.

Every query that can be written by group by and having can be written in a more complicated way by simple select-from-where idiom.

How to count number of applicants to a college?
select cName
from Apply
group by cName
having count(distinct sID) < 5;

Using sub query within having clause:
select major
from Student, Apply
where Student.sID=Apply.sID
group by major
having max(GPA) < (select avg(GPA) from Student);

The above query returns majors whose applicant's max. GPA is below the average.

LECTURE 15: NULL values
=======================

Any value of an attribute can take on the value NULL ( unspecified/unknown).

Tautology is a logical expression that is always true.

There are quite a few subtleties regarding null values and aggregation functions and null values and subqueries.
E.g.
select distinct GPA
from Student;

Returns GPAs that are null. However when you count the number of distinct GPAs, null values are not counted.

LECTURE 16: Data Modification statements
=======================================

For inserting data there are two methods --
1. Insert Into Table Values (A1, A2... An)
This inserts a single tuple into the table.
E.g. insert into College values('Carnegie Mellon', 'PA', 11500);

2. Insert Into Table
   select-statement
The select-statement produces a set of tuples, and as long as the schema of these tuples is the same as the table, they'll be inserted into the table.
E.g.
insert into Apply
select sID, 'Carnegie Mellon', 'CS', null
from Student
where sID not in (select sID from Apply);
To have all students who didn't apply anywhere apply to CS at Carnegie Mellon.

To delete existing data --
Delete From Table
where Condition

e.g.
Delete all students who applied to more than two different majors
delete from Student
where sID in
   (select sID from Apply
    group by sID
    having count(distinct major) > 2);

Some db systems don't allow (Sqlite and MySQL) delete command if you include relation you're deleting from, so it's a bit more complicated -- you have to create a duplicate temporary table and delete from the original table while querying the duplicate temporary table. PostGreSQL allows deleting from the table you're querying from.

You have to delete tuples from different relations one at a time.

Updating existing data --
Update Table
Set Attr=Expression (can involve queries etc)
Where Condition  (can involve subqueries etc)

In the set clause, the attribute is set the result of
the expression. Multiple attributes in a tuple can be
updated.

E.g.
Accept applicants to Carnegie Mellon with GPA < 3.6 but
turn them into economic majors.

update Apply
set decision = 'Y', major = 'economics'
where cName = 'Carnegie Mellon'
and sID in (select sID from Student where GPA < 3.6);

LECTURE 17: Relational Design Theory - Motivation and overview
==============================================================

Designing a database schema
- usually many designs possible
- some designs are better than others

How to choose?
- some designers use higher level tools
- useful to understand why tools provide certain schemas

There's a very nice theory for database design.

Design anomalies
* Redundancy - where we capture info multiple times
* Update anomaly - you can update facts in some places but not all
* Deletion anomaly - can inadvertently delete certain records

Instead of one big relation, divvy up the information among multiple relations. Such databases will not suffer from the anomalies with the single big relation.

The best design depends on constructing relations well but also what the data is representing in the real world.

Basic idea - design by decomposition
------------------------------------

* Start with megarelations containing everything
* Decompose into smaller relations
* Can do decomposition automatically.

Automatic decomposition:
* Mega relations + properties of the data
* System decomposes based on properties
* Final set of relations satisfies normal form.

Functional dependencies -> BCNF ( strict)
+ Multivalued dependencies -> Fourth Normal Form

General idea:
````````````
Apply(ssn, sname, cname)
* Redundancy; update and deletion anomalies
* Store ssn-sname pair for each college

Functional dependency: ssn->sName
* Same ssn always has same name
* should store each ssn's sname only once

BCNF: If A->B then A is a key.

How to decompose: Pull out key and value into a separate relation
Student(ssn, sname) Apply(ssn, cname)

Multivalued dependencies and 4NF
-------------------------------

Apply(ssn, cname, HS)
* Still has redundancy; update and deletion anomalies
* Multiplicative effect, c colleges, h high schools -- c*h tuples
We'd rather have c+h tuples.
* This badness is not addressed by BCNF as there are no functional deps.

ssn ->> cname, ssn->> HS
* given ssn has every combination of cname with hs.
* should store cname and each hs for an ssn once.

4NF: If A ->> B, then A is a key

LECTURE 18: Functional dependencies
===================================

Normal forms are good relations.

Functional dependencies are a generally useful concept
* Data storage - compression
* Reasoning about queries - optimization

Functional dependencies can have a set of attributes on each side.
* They are based on knowledge of the real world
* All instances of relation must adhere to functional dependency

Functional dependencies generalize the notion of keys. You cannot have multiple tuples with the same key.

1. Trivial FD: A -> B, if B is a subset of A.
2. Nontrivial FD: A -> is B is not a subset of A.
3. Comletely nontrivial FD: A->B where A intersection B is NULL.

Rules that apply to FD:
``````````````````````
* Splitting Rule : A -> B1, B2, B3 means A->B1, A->B2, etc.
But, you can only split the RHS.
* Combining Rule : inverse of splitting rule
* Trivial-dependency rule: If we have A->B, then we have A -> A U B. Also, if A->B then A -> A n B
* Transitive rule: A -> B and B -> C, then A -> C.
* Closure of attributes: The closure of attributes is all attributes that have dependencies on the attributes.

If closure of attributes is all attributes, it's a key.

How to specify FDs for a relation?

S1 and S2 sets of FDs. S2 follows from S1 if every relation instance satisfying S1 also satisfies S2.

How to test whether A -> B follows from S?
(1) Compute closure of A based on S. check if B in set.
(2) Armstrong's axioms - specific set of rules that are complete.

What do we want??
Minimal set of completely nontrivial FDs such that all FDs that hold on the relation follow from the dependencies in this set.


LECTURE 19: Boyce Codd Normal Form (BCNF)
=========================================

Relational design by decomposition is a way to rid the schema of anomalies.

Decomposition of a relational schema
-------------------------------------

R(A1, ... , An)
to be decomposed into
R1(B1,...,Bk)
R2(C1,...,Cm)

R1 natural join R2 = R. Also, B U C = A
R1 = projection on B attributes of R
R2 = projection on the C attributes of R

Objectives
1. We want good decompositions only -> reassembly by join returns original relation (aka lossless join).
2. Decompose into good relations that are in BCNF form

Algorithm to get good decompositions:
1. Compute keys for R.
2. Repeat until all relations are in BCNF
   i. Pick any R' with A->B that violates BCNF
  ii. Decompose R' into R1(A,B) and R2(A,rest)
iii. Compute FDs for R1 and R2
  iv. Compute keys for R1 and R2


LECTURE 20: Multivalued dependencies
====================================

4NF is much stronger than BCNF. Only a subset of FDs in BCNF are in 4NF.

E.g.
Apply(SSN, cName, hobby)

FDs ? None
Keys? All attributes of relation.
BCNF? yes, as all keys are on the LHS of the table.
Good design? 5 colleges, 6 hobbies -> 30 tuples.

Separation of independent attributes is 4NF.

For R, A ->> B means there's a multidependency between A and B.

| A| B | rest|
t|a | b1| r1  |
u|a | b2| r2  |
v|a | b1| r2  | rest value from u
w|a | b2| r1  | rest value from t

What the above tuples say is that B and rest values are independent.

Multivalue dependencies are called tuple-generating dependencies as they talk about the existence of other tuples that may not exist in the relation. For e.g. w may not exist initially, but will be generated when you establish the multivalue dependency.

But, what if we want to reveal only certain tuples?
MVDs? None
Good design? Yes.

E.g.
Apply(SSN, cname, date, major, hobby)
Reveal hobbies to colleges selectively (absence of MVD on hobby)
Apply once to each college one day (ssn, cname -> date)
May apply to multiple majors ( ssn,cname,date ->> major)

Trivial MVD: A ->> B . Either B is a subset of A or A U B = all attributes
Nontrivial MVD: otherwise

Rules for MVDs:
1. FD-is-an-MVD rule i.e. A -> B then A ->> B
2. Intersection rule: A ->> B and A ->>C, then A ->> B n C
3. Transitive rule: A ->> B and B ->> C then A ->> C - B

Splitting rule does not apply to MVDs though.

4NF => BCNF
Relation R with MVDs is in 4NF if:
   For each nontrivial A->>B, A is a key

4NF decomposition algorithm
---------------------------
Input: relation R + FDs for R + MVDs for R
Output: decomposition of R into 4NF relations with lossless join.

1. Compute keys for R
2. Repeat until all relations are in 4NF:
   i. Pick any R' with nontrivial A ->> B that violates 4NF
  ii. Decompose R' into R1(A,B) and R2(A, rest)
iii. Compute FDs and MVDs for R1 and R2
  iv. Compute keys for R1 and R2.

Ex 1.
Apply(ssn, cname, hobby)

sname ->> cname : no keys

divide into 2 relations
A1(ssn, cname) no FDs and no MVDs
A2(ssn, hobby)

Ex 2.

Apply(SSN, cname, date, major, hobby)

ssn, cname -> date ( violating FD)
ssn, cname, date ->> major (violating MVD)
no keys

A1(ssn, cname, date, major)  -> A3(ssn, cname, date) and A4(ssn, cname, major)
A2(ssn, cname, date, hobby) -> A5(ssn, cname, hobby)

LECTURE 21: Querying XML
========================

Querying XML is not nearly as mature as querying relational.
* Newer
* No underlying algebra

History
1. XPath - path expressions and conditions
2. XSLT - xpath + transformations, output formatting
3. XQuery - xpath + full featured Query language

Xlink, Xpointer - languages that use Xpath as a component.

Think of XML as a tree. Describes path expressions + conditions.

Basic constructs:
/ root element + separator
X name of the element
* matches anything
@ - attribute name prefix
// - matches any descendant of current element + self
[condition]
[n] - indexing operator

Xpath also has lots of built in functions:
1. Contains(s1, s2)
2. name() returns the tag of current element in path

Navigation axes :
There are 13 axes in xpath. It's a keyword that allows us to navigate around the xml tree.
E.g.
parent::
following-sibling::
descendant:: - matches descendants
self:: - matches current element

Xpath queries operate on and return sequence of elements.
* XML document
* XML stream

LECTURE 22: Querying XML XPath
==============================

Xpath:
/Bookstore/(Book|Magazine)/Title - all titles of books and magazines
/Bookstore/*/Title - all titles
//Title - all titles
//* - every element at every level of the tree
/Bookstore/Book/data(@ISBN) - to get all books' ISBN attributes
/Bookstore/Book[@Price < 90] - return all book elements where price is < 90.
/Bookstore/Book[@Price < 90]/Title - return titles of all book elements where price < 90.
/Bookstore/book[Remark]/Title - return titles of all book elements which have a remark element.
/Bookstore/Book[@Price < 90 and Authors/Author/Last_Name = "Widom" and Authors/Author/First_Name="Jeffrey"]/Title - the problem with this query is that if any of the authors' first name is Jeffrey and if any of the authors' last name is Widom, it satisfies the condition.
To fix the above query, we do this
/Bookstore/Book[@Price < 90 and Authors/Author[Last_Name = "Widom" and First_Name="Jeffrey"]]/Title
//Authors/Author[2] - returns the 2nd subelement
//Book[contains(Remark, "great")]/title - returns titles of all books which have a remark with the word "great".

It's possible to do "self-joins" on XML documents to compare multiple element values.

//Bookstore//*[name(parent::*) != "Bookstore" and name(parent::*) != "Book"] - all elements whose parent is not "Bookstore" or "Book".

//Bookstore/(Book|Magazine)[Title = following-sibling::*/Title] - all books and magazines with non-unique titles. But this only checks for first instance of such equivalence, so it will only return the first element that matching a succeeding one.

//Bookstore/(Book|Magazine)[Title = following-sibling::*/Title or Title = preceding-sibling::*/Title] - all books and magazines with non-unique subtitles.

//Bookstore/(Book|Magazine)[Title = following-sibling::Book/Title or Title = preceding-sibling::Book/Title] - all books or magazines with the same title as some other book.

XPath revolves around 'implicit existential quantification.' It revolves around matching sets of values and returning things if *any* element of that set matches the condition. There is no construct for 'for-all/universal' querying, where you want to see if *all* elements in the ma

The kludgy way to achieve for-all semantics is shown below --
//Book[count(Authors/Author[contains(First_Name, "J")]) = count(Authors/Author/First_Name)] - returns authors where *all* of the authors have "J" in their first name.

LECTURE 23: XQuery
==================

Querying XML is not as mature as querying databases. XPath was the original, XSLT came after that and XQuery is the newest language to query XML.

The XQuery language is compositional. You can run queries over results.Each expression operates on and returns a sequence of elements. XPath is one type of expression.

One of the commonly used expressions is the FLWOR ( flower) expression.

For $var in expr - iterator variables. The expr will produce a set of results
Let $var := expr - this is only run once, it's an assignment.
where condition - filter
Order By expr - sorts result
Return expr

* All clauses except Return are optional
* For and Let can be repeated and interleaved

It's possible to mix queries and XML.
<Result> { query goes here} </Result>

LECTURE 24: Writing in XQuery
=============================

XQuery is quite complex, certainly more complex than SQL.

To return titles of books where "Ullman" is an author.
for $b in doc("BookstoreQ.xml")/Bookstore/Book
where $b/@Price < 90 and $b/Authors/Author/Last_Name = "Ullman"
return $b/Title

To return titles and author first names of books whose title contains one of the author's first names.
for $b in doc("BookstoreQ.xml")/Bookstore/Book
where some $fn in $b/Authors/Author/First_Name
   satisfies contains($b/Title, $fn)
return <Book>
          { $b/Title }
          { $b/Authors/Author/First_Name }
       </Book>

But, the above query returns first names of all authors of books which satisfy the condition. Instead of that, if we want to return the first names of just authors that appear in the title, we run the following query --

for $b in doc("BookstoreQ.xml")/Bookstore/Book
where some $fn in $b/Authors/Author/First_Name
   satisfies contains($b/Title, $fn)
return <Book>
          { $b/Title }
          {  for $fn in $b/Authors/Author/First_Name
             where contains($b/Title, $fn) return $fn }
       </Book>

To find the average book price
<Average>
   { let $plist := doc("BookstoreQ.xml")/bookstore/Book/@Price
     return avg($plist) }
</Average>

A more compact way of writing the above query
<Average>
   { let $a := avg(doc("BookstoreQ.xml")/bookstore/Book/@Price)
     return a }
</Average>

To return all prices below the average
let $a := avg(doc("BookstoreQ.xml")/Bookstore/Book/@Price)
for $b in doc("BookstoreQ.xml")/Bookstore/Book
where $b/@Price < $a
return <Book>
          { $b/Title }
          <Price> { $b/data(@Price) } </Price>
       </Book>
data() is used to extract the data from the attribute, out of which we construct the Price sub-element.

order by - used to sort results
for $b in doc("BookstoreQ.xml")/Bookstore/Book
order by xs:int($b/Price)
return <Book>
          { $b/Title }
          <Price> { $b/data(@Price) } </Price>
       </Book>

We have to use xs:int() otherwise price will be a string, and order by will cause it to be lexicographically sorted. So, we convert it to an integer prior to sorting.

Duplicate elimination:
----------------------

for $n in distinct-values(doc("bookstoreQ.xml")//Last_Name)
return $n

But, the above query strips the element names from the result.

for $n in distinct-values(doc("bookstoreQ.xml")//Last_Name)
return <Last_Name> { $n } </Last_Name>

Leaving out the curly braces around $n will cause the result to print "$n". Instead, to evaluate $n use curly braces.

Universal querying
==================

Returns all books which have authors with first names containing the letter "J"
for $b in doc("BookstoreQ.xml")/Bookstore/Book
where every $fn in $b/Authors/Author/First_Name satisfies contains($fn, "J")
return $b

LECTURE 25: XSLT
================

Extensible Stylesheet language. T is for transformations, which was added later. XSLT is in XML format.

XSLT: rule-based transformtions

* Match a template and replace
* Recursively match templates
* Extract values
* Iteration (for-each)
* Conditionals (if)

Drawbacks:
* Strange default/whitespace behavior
* Implicit template priority scheme

When you have elements that do not match the template, XSLT returns a concatenation of all string leaf values.

<xsl::template match="Book" />
When you find a Book element, throw it away

When you have two templates that both match, there are some inbuilt priorities that determine which one overrides the other.

<xsl:template match="*|@*|text()">
   <xsl::copy>
      <xsl:apply-templates select="*|@*|text()" />
   </xsl:copy>
<xsl:template>
Does recursive application of templates.

LECTURE 26: UML Data Modeling
=============================

Unified Modeling Language

Data Modeling: How to represent data for application
* Relational model
* XML

Database design model ( higher level model that is translated into relations or XML)
- Not implemented by system
- Translated into model of DBMS

*Entity relationship model (E/R) was the de facto model for database design
* UML has been popular lately

Both are graphical models which can easily be translated to relations.

5 concepts - classes, associations, association classes, subclasses, composition & aggregation.
* Associations capture relationship between classes.
* Multiplicity of associations between classes is possible. ( complete multiplicity can be applied to one-to-one, many-to-one, one-to-many and many-to-many).
* Association classes allow us to put attributes on association. What we cannot describe in UML we can't describe more than one relationship between two objects.

How to eliminate association classes?
* Unnecessary if 0...1 or 1...1 multiplicity

It is possible to have self associations -- between a class and itself.

It's okay for subclasses to have no attributes.

generalization -> superclass
specialization -> subclass
partial/complete (every obj in superclass is in atleast one subclass)
disjoint(every object is in at most one subclass)/overlapping

Composition -- all objects of one class belong to objects of another class. (1..1)
Aggregation -- only some objects of a class belong to objects of another class  (0...1)

LECTURE 27: UML To Relations
============================

Database design model is translated to a database schema.

Designs can be translated to relations automatically provided every class has a regular key.

Association grabs a key from each side. Every association is converted to a relation, and the key for this relation depends on the multiplicity. If we have 0..1 or 1..1 on one side of the association, the other side's key is the key for the corresponding relation. If there's no 0..1/1..1 on either side, keys from both relations will be included as keys in the association relation.

Sometimes, association relation is not always required. It's possible to fold the association relation into one of the classes.  If we have 0...1 or 1..1 on one side, add the key from the 0..1/1..1 relation to the one on the other side. We won't need an association relation now.

Association class: just add attributes to relation you're generating anyway.

Require a key for every "regular" class. What does this mean?
Association classes may not be regular as they may not have a key.

Self associations are possible.

Subclasses -
Many possible translations, so best translation depends on properties
1) Subclass relations contain superclass key + specialized attrs
2) Subclass relations contain all superclass attributes
3) One relation containing all superclass + subclass attrs

Heavily overlapping: design 3.
Disjoing, complete: design 2

Subclasses and association classes need not be regular classes with a single key.

LECTURE 28: Indexes
===================

* Indexes are the primary way to get improved performance on a database.
* Persistent data structure used in a database.

Users don't access indexes, they're used underneath the query execution engine.

Tables can be absolutely gigantic in a database, and indexes make a huge difference in performance.

Underlying data structures
- B-trees ( A=V, A < V, between values, etc.) - logarithmic
- hash tables can only be  used for equality. - constant running time.

Many database systems build indexes based on PRIMARY KEY ( and sometimes UNIQUE) attributes.

Query planning and optimization are the most interesting area of query languages these days.

Downsides of indexes:
1. Extra space - marginal downside
2. Index creation overhead - medium downside
3. Index maintenance - index should be up to date with database. For a db that is modified too often, index may offset the benefits.

Benefit of an index depends on:
* size of table
* data distributions
* Query vs, update load

Physical design advisor:
Input = database(statistics) and workload
Output = recommended indexes
It experiments with different types of indexes and selects those indexes where the benefits outweigh drawbacks.

LECTURE 29: CONSTRAINTS AND TRIGGERS
====================================

* SQL standard; systems vary considerably.

Constraints - constrain allowable database states (static concept)
Triggers - monitor database changes, check conditions and initiate actions (dynamic concept)

Integrity constraints:
- impose restrictions on allowable data, beyond those imposed by structure and types
e.g. 0 < GPA <= 4.0
We can have fairly complicated constraints involving multiple attributes.

Why use constraints?
- data entry errors (inserts)
- correctness criteria (updates)
- enforce consistency
- tell the system about the data so that it can store data and process queries efficiently.

Classification of integrity constraints:
1. non-null : that values cannot take on null values
2. key: a column or set of columns must have unique values in tuples
3. referential integrity ( foregin key)
4. Attribute-based
5. Tuple-based ( how attributes in a tuple relate to each other)
6. General assertions (allow you to set constraints across entire db).  Not implemented yet in any DBMS.

Declaration of constraints:
1. Declare with original schema - so constraints are checked after bulk loading.
2. Or later - checked on the current state of the DB

Enforcement:
1. Check after every "dangerous" modification
2. Deferred constraint checking - check after every transaction. A transaction is a group of nodifications executed as a single unit.

Triggers "Event-Condition-Action Rules"
When event occurs, check condition, if true do action
E.g.
enrollment > 35000 -> reject all applicants
insert app with GPA > 3.95 -> accept automatically
update sizeHS to be > 7000 -> change to "wrong" and raise an error.

Why use triggers?
Original motivation to move logic from apps to DBMS.
To enforce constraints in an expressive manner.
It's also possible to have constraint "repair" logic to launch an action that fixes the constraint.

LECTURE 30: Constraints of several types
========================================

Non-null constraint: we just add keyword 'non-null' to the declaration

Key constraint: the attribute which is declared a key must only have unique values among all tuples.

Multi-attribute keys are also possible. You can also specify sets of attributes as "unique." Null values dont count as keys. SQL standard allows multiple null values even in columns declared unique.

Attribute check constraint: apply value-based constraints on attributes. This is done to prevent data entry errors.

General assertions: not supported by any DBMS. For every possible change that can violate the assertion, the system must check for the assertion.

LECTURE 31: Referential integrity
=================================

Refers to integrity of references = no "dangling" pointers.

Referential integrity from R.A to S.B means that each value in column A must appear in column B of table S. Referential integrity is directional.

The referencing attribute is called a 'foreign key'. B is required to be the primary key or atleast unique. Also, multi-attribute foreign keys are allowed.

If we have an update/insertion to referencing table, it can cause a violation. Similary if we're modifying the referenced table.

Special actions for update/delete:
Delete
* Restrict(default action) returns an error
* Set Null (set attributes to null in referencing table)
* Cascade (Delete tuples that reference that particular value)

Updating attributes has similar actions as above. Except, cascade will propagate to all referencing tables.

Can have referential integrity within a table.

LECTURE 32: Triggers
====================

Triggers: event-condition-action rules.

1) Moves monitoring logic from apps to DBMS.
2) Enforce  integrity constraints.
* Beyond what constraint system supports
* Automatic constraint repair

* Implementations vary significantly.

Triggers in SQL:
Create Trigger name
Before|After|Instead Of events - insert/delete/update
[referencing variables] - give us a way to modify data
[For each row] - optional clause that specifies that trigger should be activated for each tuple.
When(condition) - like a SQL where condition, like a general assertion
action - SQL statement

Some systems dont support row-level or statement-level reference variables, so you have to use either.

Multiple triggers can be activated at the same time, which goes first depends on the system implementation. Trigger actions activating other triggers (chaining) - also self-triggering, cycles, nested invocations.

Condition in 'when' vs. as part of 'action' - this could affect efficiency.

LECTURE 33: Triggers Demo
=========================

Primary open-source systems
* Postgres - implements full standard, but uses cumbersome syntax
* SQLite - row level only, immediate activation -> no old/new table
* MySQL - row-level only, immediate activation
        - only one trigger per event type
        - limited trigger chaining

Can activate trigger before or after (default).

LECTURE 34: Triggers (continued)
================================
Postgres > SQLite >> MySQL

Triggers can trigger themselves. You can turn this behavior by activating recursive triggers in SQLITE.

LECTURE 35: Transactions
========================
Motivations -
1. Concurrent database access by multiple clients
2. Resilient to system failures

Concurrent accesses can result in
i. Attribute-level inconsistency - see both changes or either change
ii. Tuple level inconsistency - see both changes or either
iii. Table level inconsistency

Goal: Execute sequence of SQL statements so they appear to be running in isolation.

We only want to enable concurrency whenever safe to do so.

Solution to both concurrency and failures: Transactions
* A transaction is a sequence of one or more SQL operations treated as a unit.
* Transactions appear to run in isolation
* Either all complete or none at all

LECTURE 36: Properties of Transactions
======================================

ACID properties - Atomicity, consistency, isolation, durability

ISOLATION
Serializability - operations within transactions may be interleaved, but execution must be equivalent to some sequential order of all transactions. This is possible by locking portions of databases.

Database systems only guarantee serializability i.e. SOME sequence of transactions, not any specific order of transactions.

DURABILITY
If system crashes after transaction commits, all effects of transaction remain in the database. This is made possible by logging.

ATOMICITY
Each transaction is "all or nothing." Also uses logging mechanism.

Transaction rollback (=abort) is the implementation mechanism behind atomicity.
* Undoes partial effects of transactions
* Can be system- or client-initiated

CONSISTENCY
* Can assume all constraints hold when transaction begins
* Must guarantee all constraints hold when transaction ends

LECTURE 37: Transaction isolation
=================================

Serializability's drawbacks
- overhead
- hurts concurrency

isolation levels ( in order of weak to strong)
- read uncommitted
- read committed
- repeatable read
- serializable

Isolation levels are on a per transaction basis, and they are specific to reads.

Dirty data item: Written by an uncommitted transaction
Read Uncommitted: A transaction may perform dirty reads. Results in increased concurrency.

Read committed: A transaction may NOT perform dirty reads. But this still does not guarantee global serializability.

Repeatable read: A transaction may not perform dirty reads. An item read multiple times cannot change value. But a relation can change: "phantom"  tuples. Some systems have this as default (Oracle, MySQL).

Read Only transactions: Helps system optimize performance. Independent of isolation level.

LECTURE 38: VIEWS
=================

Views are based on a 3 level vision of databases.
Physical - Conceptual - Logical

Actual disks - physical
Conceptual - relations
Logical - views ( query over relations, over other views, etc.)

Why use views?
- hide some data from users
- make some queries easier/more natural
- modularity of database access

Real applications tend to use lots and lots of views. Once they are defined, they can be referenced like any table.

View V = ViewQuery(R1m R2,...,Rn)
Schema of V is schema of query result.

Syntax for creating a view:
Create View Vname(A1, A2,...,An) As
<Query>

LECTURE 39: View Modifications - introduction
=============================================

Views aren't stored, so modifying them is counterintuitive. But, views are some users' entire view of the database.
Solution: Modifications to V rewritten to modify base tables.

(1) Rewriting process explicitly specified by view creator ( instead-of triggers)
+ can handle all modifications
- no guarantee of correctness ( or meanigful)

(1) Restrict views + modifications so that translation to base table modifications is meaningful and unambiguous. (SQL standard)
+ no user intervention
- restrictions are significant

LECTURE 40: View modifications thru triggers
============================================

'instead of' is used to imply that instead of deleting tuples from a view, delete from/insert into the underlying base table.

A view is not a relation, it's only created by running queries on base tables.

Views can hae ambiguous modifications where you want to prevent users from modifying relations.
1. if a view is based on aggregate data, then it doesn't make sense to modify underlying data.
2. Also when you use 'distinct' in the view query.
3. If there's a complicated self-referential query expression.
4. If there are multiple tables in the FROM clause.

LECTURE 41: Automatic View Modifications
========================================

* Once V is defined, want to modify V like any other table
* Modifications to V rewritten to modify base tables.
* Unlike queries to views, modifications cannot be automated.

1. Rewrite process to update Views specified explicitly by view creator
+ can handle all modifications
- no guarantee of correctness

2. Restrict views + modifications so that translation to base table modifications is meaningful and unambiguous.
+ no user intervention
- restrictions are significant. They're also called "updatable views."

Restrictions:
1. Select  (no DISTINCT) on single table T
2. Attributes not in view can be NULL or have default values
3. Subqueries must not refer to T
4. No Group By or aggregation

We don't want updates to view that affect the base table but the changes do not manifest on the view.

with check option - when the system automatically performs an operation, it checks if the tuple to be inserted conforms to the base table schema.

LECTURE 42: Materialized Views
==============================

Virtual views are what we've seen so far. They're just a query on the database.

A materialized view gives additional advantage - improve database performance. It's actually a table in the database.

Problems
* The view could be very large.
* Modifications to base tables => recompute or modify view.

Incremental maintenance algorithms can be used to piecewise update materialized views.

Modifications on materialized views
+ just update the view table
- base tables must also be modified

Picking which materialized views to create
* Size of data
* Complexity of view
* Number of queries using view
* Number of modifications affecting view
Basically, the above problem is a query-update tradeoff. Very common tradeoff when dealing with databases, for e.g. when dealing with indexes.

Automatic query rewriting to use materialized views
---------------------------------------------------
Possible for a sophisticated DBMS to reuse views in place of performing joins on base tables.

LECTURE 43: Authorization
=========================

* Make sure users see only the data they're supposed to see
* Guard the database against modifications by malicious users

+ Users have privileges and can only operate on data for which they're authorized.

You can even apply authorization to views.

Obtaining privileges: Relation creator is owner. Owner has all privileges and may grant privileges.

Grant privs on R from users [options]
Revoke privs on R from users [cascade|restrict]
Cascade to other users granted by user. Restrict means disallow if cascade will revoke any other privileges.

End users may not be interacting with privilege system of database.

LECTURE 44: Recursion in SQL - Basic recursive WITH statement
=============================================================

SQL is not a turing-complete language
* Simple, convenient, declarative
* Expression enough for most database queries
* Can't express unbounded computations

Examples of unbounded computations:
Example 1: Finding Ancestors - ParentOf(parent, child)
Example 2: Company hierarchy - project cost given hierarchy
Example 3: Airline flights - find cheapest way to fly from A to B

SQL 'with' statement - adds notion of recursion.
With R1 as (query-1),
     R2 as (query-2),
     ...
     Rn as (query-n)
<query involving R1,...,Rn (and other tables)>

E.g. of recursive query

with recursive
   R as (base query union recursive query)
<query involving R (and other tables)>

base query seeds the table and the recursive query continues to add tuples to the table until there are no tuples to add.

LECTURE 45: Recursion: Basic recursive WITH Statement
=====================================================

1. To find ancestors
with recursive
Ancestor(a,d) as (select parent as a, child as d from ParentOf
                  union
                  select Ancestor.a, ParentOf.child as d
                  from Ancestor, ParentOf
                  where Ancestor.d = ParentOf.parent)
select a from Ancestor where d='Mary'
# finds Mary's ancestors

2. To find project cost
with recursive
Superior as (select * from Manager
             union
             select S.mID, M.eID
             from Superior S, Manager M
             where S.eID = M.mID)
select sum(salary)
from Employee
where ID in
   (select mgrID from Project where name = 'X'
    union
    select eID from Project, Superior
    where Project.name = 'X' and Project.mgrID = Superior.mID)

When we think of recursion we think of transitive closure.

3. lowest cost flight plan
with recursive
Route(orig,dest,total) as
   (select orig, dest, cost as total from Flight
    union
    select R.orig, F.dest, cost+total as total
    from Route R, Flight f
    where R.dest = F.orig)
select * from Route
where orig = 'A' and dest = 'B'

The above query works, but it results in infinite recursion. You can break the recursion by adding a check to the query to only add routes with a cost < total for same route (from tuples that already exist in table). Or use a limit feature.

LECTURE 46: Nonlinear and mutual recursion
==========================================

Joining a table with itself in the recursive query is an example of non-linear recursion.

+ looks cleaner
+ converges faster
- harder to implement
SQL standard only required linear.

Mutual recursion: Recursively defined relations refers to one another.

Hubs & authorities >> competition to page rank
Some Hubs and authorities are pre-designated. Hubs are linked to >= 3 Auuths. Auths are linked to >= 3 hubs.

Recursion with aggregation is disallowed as there could be ambiguity.

LECTURE 47: OLAP ( On-line Analytical Processing)
=================================================

Database processing is broadly divided into two categories-

OLTP (online transaction processing)
- short transactions
- simple queries
- touch small portion of data
- frequent updates

OLAP (online analytical processing)
- long transactions
- complex queries, data mining type operations
- touch large portions of data
- infrequent updates

Systems are tuned for both kinds of operations.

* Data warehousing - bring data from OLTP sources into a single warehouse for OLAP analysis.
* Decision support system (DSS) - infrastructure for large scale data analysis. e.g. data warehouse tuned for OLAP.

"Star Schema" contains
* 1 Fact table - updated frequently, often append-only, very large
e.g. sales transactions, course enrollments, page views
* Dimension tables - updated infrequently, not as large
- stores, customers, items
- students, courses
- web pages, users, advertisers

Fact table references dimension tables. They contain dimension and dependent attributes.

OLAP queries: Join -> Filter -> Group -> Aggregate

Performance:
- Inherently very slow: special indexes, query processing techniques
- Extensive use of materialized views

Data Cube (a.k.a multidimensional OLAP)
* Dimension data forms axes of cube
* Fact (dependent) data in cells
* aggregated data on sides, edges and corners

Drill-down and roll-up
----------------------

drill down - Examining summary data, break out by dimension attribute. We add a grouping attribute to get more detail.
roll up - examining data, summarize by dimension attribute.

SQL constructs: With cube and With Rollup

LECTURE 48: OLAP ( On-line Analytical Processing) continued
===========================================================

Only MySQL supports With Rollup, but not With Cube.

For drilling down, we add more attributes to the group by clause.
Slicing - invokes the data cube. Done by constraining query.
Dicing - slice in two dimensions.
Rolling up - take attributes out of group by clause, as we want less detail.

with cube - data in data cube.
with rollup can be used to effectively produce 'with cube'

If attributes are hierarchical e.g. state, county, city, with rollup is useful.

LECTURE 49: NoSQL (Motivation)
==============================

Misleading name. "SQl" referred to here is traditional relational DBMS, it doesn't refer to the SQL language.

Not every data management/analysis problem is best solved using a traditional DBMS.
* Higher efficiency requirements
* massive databases
* fewer transaction guarantees required -> relaxed consistency with higher performance & availability
* Quicker and cheaper to set up
* Flexible schema

Downsides
* no declarative query language -> more programming involved
* Relaxed consistency -> fewer guarantees

Therefore, accepted definition = "Not only SQL".

Example 1. Web log analysis
----------------------------
Each record: UserID, URL, timestamp, additional-info

Task 1:load data into system
Data cleaning is required for traditional DBMS. E.g. timestamp has to be in standard format.
Data extraction to format data in additional info field to extract essential details.
Schema specification
Verification

All of the above is not mandated by a NoSQL system, so it makes it easier to add records.

Task 2: find all records for..
* Given UserID
* Given URL
* Given timestamp
* Certain construct appearing in additional-info

The above operations are highly parallel.

Task 3: Find all pairs of UserIDs accessing same URL
Requires a self-join, but it's a slightly rare query so noSQL may not support it.

Task 4:Find average age of user accessing given URL
SQL-like


Example #2: Social network graph
--------------------------------

Each record: UserID1, UserID2
Separate records: UserID1, name, age, gender,...

* Operations may require a large number of joins, so it's not suitable for SQL
* approximate solutions are acceptable, so consistency can be weak


Example #: Wikipedia pages
--------------------------
Large collection of documents
Combination of structured (key-value pairs) and unstructured data (large volumes of text).

* Consistency is not critical
* SQl is not the best solution as we may have to search over structured and unstructured data.

LECTURE 50: NoSQL (continued)
=============================

Several incarnations of NoSQL systems
- MapReduce ~ OLAP applications
- Key-value store ~ OLTP application - lot of small operations touching small portion of data
- Document stores
- Graph database systems

Column stores are another way of organizing data in relational databases for higher performance access. Key-value stores are like column stores so you'll hear the term column store frequently when NoSQL is being dicussed.

MapReduce framework:
* Designed for OLAP operations
* Original from Google, open source Hadoop
* No data model at all, data is stored in files (e.g. GFS, HDFS)
* User provides specific functions
- map() <-> reader()
  reduce() <-> writer(), combiner() [a per-reduce phase]
* System provides all the execution glue.

Map: takes a data item as input and generates zero or more key-value pairs

Reduce - takes a key and list of values to produce 0 or more methods.

Drawbacks of Map-Reduce:
-----------------------
Schemas and declarative queries are missed.
Hive - schemas, SQL-like query language
Pig - more imperative but with relational operators
* Both languages compile to "workflow" of Hadoop(MapReduce) jobs.
Dryad allows user to specify workflow.
* also DryadLINQ language.

Key-Value stores
----------------
* Extremely simple interface
* Designed for OLTP operations
* Data model: (key, value) pairs
* Operations: insert, fetch, update, delete

Efficiency, scalability and fault-tolerance are desired.
* Records distributed to nodes based on key.
* Replication
* Single-record transactions, "eventual consistency" i.e. replicas may diverge from one another, but if all operations stop, replicas will eventually become consistent.

* Some key-value stores have columns, which means the value is actually structured. Some allow columns within value.
* Some allow Fetch on a range of keys.
* Example systems - Google Big Table, Cassandra, Amazon Dynamo.

Document Stores
---------------
* Like key-value stores except value is document.
* Data might contain JSON, XML, other semistructured formats.
* Basic operations - insert, fetch, update, delete
* Also fetch based on document contents
* Example - couchDB, MongoDB, SimpleDB.

Graph database systems
----------------------
* Data model: nodes and edges
* Nodes may have properties ( including ID)
* Edges may have labels or roles.

Drawbacks: no standardization at all. Interfaces and query languages vary.
Examples: neo4j, flockdb, pregel
RDF "triple stores" can map to graph databases.
