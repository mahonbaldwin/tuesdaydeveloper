---
layout: post
title: "Data Isolation and Database Locking"
date: 2009-11-21 10:00:00.0
author: mahon
categories: 
---
<div>There are several problems that can occur on a relational database when muliple users are accessing the same data.</div>
<div style="margin-left: 25px; "><strong>Dirty Read:</strong> (No, it's not a romance novel.) When a record is changed by "Person A" but hasn't yet been committed. "Person B" reads the data that hasn't been updated. It can also occur when "Person B" reads data that ins't committed to the database, if "Person A's" transaction is rolled back, the data that Person B has read is no longer valid.</div>
<div style="margin-left: 25px; "><strong>Example:</strong> (numbers are indicative of time intervals) Two people share a bank account, "A" wants to pay a bill which requires the called method to debit and then credit their account record in the database.</div>
<div style="margin-left: 25px; ">1- "A" reads that there is $300 in his bank account.</div>
<div style="margin-left: 25px; ">2- "A" starts to pay a bill and his account is debited by $150</div>
<div style="margin-left: 25px; ">3- "B" looks at the account balance and reads that there is $150</div>
<div style="margin-left: 25px; ">4- An error occurs when "A's" transaction is processing the credit to the payees table and the transaction is rolled back. The account balance is now $300 again.</div>
<div style="margin-left: 25px; ">This is an example of a dirty read because the information that "B" has is no longer valid.</div>
<div style="margin-left: 25px; "><strong>Non-repeatable Read:</strong> Is the same as a dirty read but it involves a second record call from the database, if the second time the same record different, it is a non-repeatable read. (Up until the data is changed, it is still a dirty read.)</div>
<div style="margin-left: 25px; "><strong>Example:</strong> "A" and "B" share an account. "A" wants to pay a bill which requires the called method to debit and then credit their account record in the database.</div>
<div style="margin-left: 25px; ">1- "A" reads the account balance, $300</div>
<div style="margin-left: 25px; ">2- "B" reads the account balance, $300</div>
<div style="margin-left: 25px; ">3- "A" debits the account balance, it is now $150</div>
<div style="margin-left: 25px; ">4- "B" reads the account balance again and the value has changed</div>
<div style="margin-left: 25px; "><strong>Phantom Read: </strong>When a record is deleted or a new record is added to a database that effects what another user is doing.</div>
<div style="margin-left: 25px; "><strong>Example:</strong> "A" is a client for an online store, "B" is the manager for the store.</div>
<div style="margin-left: 25px; ">1- "A" reads that there is 3 widgets in inventory.</div>
<div style="margin-left: 25px; ">2- "B" receives a shipment and updates the inventory of the widgets to 5003</div>
<div style="margin-left: 25px; ">3- "A" hurries to by before there are none left, but when he is done there is over 5000 widgets in the store.</div>
<div>These problems call for transaction isolation. Transaction isolation restricts how users can access data in the database. There are four different levels for Java EJBs:</div>
<div style="margin-left: 25px; ">Read Uncommitted: This is the lowest level of isolation, it allows dirty reads, non-repeatable reads, and phantom reads. This level of isolation can be used when you are not worried about collisions in the data and you're willing to sort them out if any.</div>
<div style="margin-left: 25px; ">Read Committed: A transaction cannot read uncommitted data, a record (or a table depending on your DBMS) will be locked and that data cannot be read by other transactions. This prevents dirty reads, but allows both non-repeatable reads and phantom reads.</div>
<div style="margin-left: 25px; ">Repeatable Read: This level of isolation prevents one transaction from changing data that is being read by another transaction. This prevents both dirty reads and non-repeatable reads, but phantom reads can still occur.</div>
<div style="margin-left: 25px; ">Serializable: Only one transaction has read and write privileges at a time. This prevents all read/write problems, but may cause performance to be slow for its users.</div>
<div>Just like anything else, there is give an take with what is ideal and what can occur in an application that interacts with a database. In general I think that users are understanding that other users can have an effect on the data, but it is still something you should plan for. Perhaps if one user is viewing data that another user is updating it would be appropriate to indicate that there is something going on there so that the users understand why their transaction is taking so long, or at least to let them guess that their data may have changed.</div>
<div>Resources: Enterprise JavaBeans 3.0 5th Edition 2006 Bill Burke &amp; Richard Monson-Haefel</div>