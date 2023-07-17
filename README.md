# EOS is written in C++, so its good to have a basic understanding of the language
# As of July 2023, EOS has different IDE for Windows and iOS/Linus.  
# For Window, uses EOS Studio Web which can be access using the following link
## https://www.eosstudio.io/
## https://app.eosstudio.io/
# For iOS/Linux, EOS Studio can be download and install into the machine directly, it also need Docker installed to run.  For more information, need to google it.
#
# -----------------------------------------------------------------------------------
# Interesting Medium article explaning the basic architecture of EOS blockchain:
# https://medium.com/fueled-engineering/exploring-the-eos-multi-index-database-557769b1b7a6
#
# -------------------------------------------------------------------------------------------------------------------
# Setting up EOS studio project: 
# 1. When creating a EOS project several things needs to take into considerations:
#   a. It must have a local path (for iOS and Linux only), for EOS studio Web (Window) local path is automatically set.
#   b. Project Name have certain constrains, which are:
##      i. 12 characters or less
##      ii. only lowercase letters 
##      iii. numbers between 1~5
##      iv. can have full stops (.)
#
# Upon successfully creating a project, we can open it, and it should look similar to VS community project enviroment.
#
# 2. After creating a project, we need to choose a Network to work on.  For iOS/Linux, create a local network node.  For Windows, use one of the testnet network.
#
# 3. If haven't done before, setup up some keypairs in EOS Studio, by presing the key shape button.  Then click create, then save.  This would generate a new keypair.
#
# --------------------------------------------------------------------------------------
#
# When creating a constructor class (typically in the hpp file) it is important to have the name same as the project name.  e.g. if the project name is called 'helloworld' then the constructor class also needs to be name 'helloworld'
#
# To write a Smart Contract in EOS, we need to define it like a class, but with the keyword 'CONTRACT' instead of 'class'.  Also CONTRACT must public inhereit from the 'eosio::contract' library, also needs to call the library in the public constructor. And finally, to use the contract, we need to define a function where it shall be used, and use the keyword 'ACTION' instead of keywords such as 'void'. for example:
# NOTE: to use CONTRACT and ACTION, needs to include the library in the .hpp file
## #include  <eosio/eosio.hpp>
##  CONTRACT helloworld : public eosio::contract
## {
##      public:
##          using eosio::contract::contract;
##          ACTION hi(eosio::name const &nm);
## };
#
# Below is a typical C++ constructor example, so we can use it as a comparison example of how similar it is to a EOS smart contract:
##  class helloworld :
## {
##      public:
##          void hi(dosomething);
## };
# 
# ------------------------------------------------------------------------------------
#
# To compile the code, click on the hammer icon
# Whearas to deploy the code, click on the ship icon
#
# ------------------------------------------------------------------------------------
#
# Tables are used to store persistent data in EOS.  That is to simply put, if global variables in smart contract does not retain its updated value after it has run through, thus everytime the program runs, it will be the value that it initialised as.  Therefore in order to keep any modifications, we need to store it in a table instead.
#
# A Table in EOS, is actually a C++ struct.  An example:
# struct{
#    id:
#    name:
#    address:
# };
#
# the struct are then initialise and turn into a table.  an example of instantiate struct:
#
# People (name of the newly initialised struct/ table)
# id  |  name  |  address
# -------------------------
# 1   |  alice |  alice@gmail.com
# 2   |  bob   |  bob@gmail.com
#
# Once a table is created we can only: add, remove, modify the data.  As well as several other functions such as search and lookup.  That is once an instance is instantiated, we can't change the paramaters in the struct, unless we delete the instance, modify the struct and then re-instantiate the instance.
# Therefore it is important to remember in the design phase, is consider all the parameters and data needed to be stored in the table.
# When creating a table, we must include 'Primary Key' into it.  A Primary Key that act as a unique identifier, thus it won't have any duplicate and cause confucsion.  In addition, we must let EOS know which coloum is the designated Primary Key, in the example above, id would be the designated primary key.
#
# when a table is created it can access by:
# people(accounts, scope)
# where:
# account : where the table is hosted, i.e. the account the smart contract is deployed to where the table is.
# scope : the view in which how to view the table.  Three different way to access table using scope:
##  1. Global: 1 table with 1 scope. Always access the same table everytime
##  2. Groups: 1 account with multiple scopes.  I.e. People account could have two tables to store data, male and female, but all belows to the People account.  Thus depends on the scope being called, we can view and/or mainuplate either male, female or both data in the table.  To use this it can be called using something like:
###     people(people, male);
###     people(people, female);
##  3. Individual: using the sender account name or account of interest as the scope to call the table. for example:
### people(people,requester-wallet-address)
#
# -------------------------------------------------------------------------------------
#
#