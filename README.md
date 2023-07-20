# EOS is written in C++, so its good to have a basic understanding of the language
# As of July 2023, EOS has different IDE for Windows and iOS/Linux.  
# For Window, uses EOS Studio Web which can be access using the following link
## https://www.eosstudio.io/
## https://app.eosstudio.io/
# For iOS/Linux, EOS Studio can be download and install into the machine directly, it also need Docker installed to run.  For more information, need to google it.
#
# -----------------------------------------------------------------------------------
# Interesting Medium article explaning the basic architecture of EOS blockchain:
# https://medium.com/fueled-engineering/exploring-the-eos-multi-index-database-557769b1b7a6
#
# -------------------------------------------------------------------------------------
#
#   IMPORTANT REMINDER: Although EOS is developed using C++, however EOS studio and EOS CLI were made especially to run on Apple and/or Linux machine.
# Thus if you are like me, who has and uses a Window's machine, we must install WSL (Windows Subsystem for Linux) and create a Linux Virtual Machine (VM) using it.  I started the journey using Ubuntu 20.04 (July 2023), however I found out the EOS cli even though they claim to be able to support Ubuntu 20.4, however alot of the required libraries needed to setup EOS cli is obseleted and/or no longer support Ubuntu 20.4.
# The offical Microsoft guide to install WSL is (there's also plentiful of good forum, blogs and YouTube teaching how to install also):
#   https://learn.microsoft.com/en-us/windows/wsl/install
#
# -------------------------------------------------------------------------------------
# 
#   To setup EOS cli without using using EOS studio, use the following guideline from EOS's official webpage:
# https://developers.eos.io/welcome/v2.0/welcome-to-eosio/index
# https://developers.eos.io/welcome/v2.0/getting-started-guide/local-development-environment/index
# NOTE: as of writing this cheatsheet (July 2023), the guide suggested EOS can be setup using Ubuntu 20.4, Ubuntu 18.04 and UBUNTU 16.04.  However, when I tried to install the EOS environment and get nodeos running using Ubuntu 20.04 (latest Ubuntu version at the time of writing), a lot of support libraries are obseleted and/or does not support Ubuntu 20.04.  Thus will retry the installing process using Ubuntu 18.04.  
# UPDATE on NOTE above: I tried reinstalling the EOS cli environment using Ubuntu 18.04, it seems to be able to install all the libraries and set up the environment correctly, without any warnings and errors.  Thus, I personally suggest to work on Ubuntu 18.04, and hope in the future the community will update the code to support lastest version of Ubuntu.
# UPDATE 2: As of July 2023 although I can install the EOS enviornment on the WSL version of Ubuntu 18.04, I still can't get nodeos to run on it.  Therefore, if whoever is reading this wants to get EOS environment working, I would suggest you to do it on a Apple or Linux OS machine.  Unless in the future the community had work around a solution to get everything working on WSL image of a Linux Machine, otherwise using a Window machine work with EOS is HIGHLY NOT RECOMMANDED.
#
# --------------------------------------------------------------------------------------
#
# To create a wallet, after EOS cli environment had been setup (use guide above to help setup, if needed), use the following:
##  cleos wallet create --to-console
# The command above will also display the password for the default wallet created using the cleos command.  Need to save the password safely somewhere, if we want to interact the wallet after creating it.
#
# IMPORTANT: If you want to use the wallet you've just created for REAL transaction with REAL tokens and monatery values, PLEASE DO NOT SAFE THE PASSWORD ANYWHERE THAT ARE EASILY ACCESSIBLE BY THE PUBLIC. OTHERWISE YOU'LL LOOSE THEM.  The password for the wallet I saved in this project, I created the wallet with the intention of using testnet tokens, which have no monatery values.
#
# After creating the wallet, open the wallet using:
##  cleos wallet open
# After opening the wallet, we can list all the wallets created using:
## cleos wallet list
# To unlock a wallet use the following:
## cleos wallet unlock
### NOTE: code above unlocks the default wallet (when prompt, we need to insert the password for the wallet to unlock it).  If you created multiple wallet and want to open other wallet that isn't the default wallet, use the following:
## cleos wallet unlock -n <NAME_OF_WALLET>
### NOTE: Replace <NAME_OF_WALLET> with the name of the wallet you want to unlock, e.g/ cleos wallet unlock -n tablewallet
#
# Creating an EOS account and add keypair to the wallet, by using the following:
##  cleos create key --to-console
# Import the keypair to the wallet, which requires to add the eosio account.  Use the following code to import keypair, use the privatekey (from creating keypair just above):
## cleos wallet import
### NOTE: when performing this step, make sure the to unlock the wallet we want to import the account to first.
#
# After importing the keypair from above we also need to import the EOSIO Development key, which can be done using the following:
##  cleos wallet import
##  use the folloiwng private key as the Development Key: 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
#
# Creating an account, by using the following:
##  cleos create account eosio phil EOS8jM4zNksEQURyyDta4U6Hi3brisLXcqi3U5SvatieC8RNVe5GT EOS8jM4zNksEQURyyDta4U6Hi3brisLXcqi3U5SvatieC8RNVe5GT
### NOTE: when creating an account, we need to specify the account to pay for the creation of the account.  If this is the first time creating an account, EOS addmin account pay for it, thus we use the name 'eosio'.
### NOTE2: phil is the name of the account we just created using cleos create account eosio phil
### NOTE3: The first key is the public key for the active permission, the second key is the public key owner permission. In the code above I use public key from the owner account I created from the step above, because I'm experimenting EOS developmet environment on a local node.  Thus if you want to create a real project on the mainnet or testnet, you should provide two separate public key (one for active wallet and the other the owner wallet)
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
# NOTE: ACTION keyword is use to declare the function in the smart contract, but the actul function itslef is declare like a normal C++ function.  for example:
##  In the helloworld.hpp we declare the function using:   
###  ACTION addpet();
##  While in the main.cpp file, we code the function using:  
###  void addpet(){}       
#
# -------------------------------------------------------------------------------------
#
# To let only a certain person to interact with item in the table, we can use:
## require_auth();  //e.g. require_auth(owner)
# To check does the action about to be perform meet the logcial requirement before proceeding with the action (e.g. item exist in the table before modification is allowed), we can use the check function.  e.g.:
##  eosio::check(pet_itr != pets.end(), "The ID you have enetered does not exit") 
#
# ------------------------------------------------------------------------------------
#
# In EOSIO studio there's a existing project called eosio.token, in whcih it has all the functions needed to make a dapp, as well as creating tokens
#