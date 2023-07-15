# EOS is written in C++, so its good to have a basic understanding of the language
# As of July 2023, EOS has different IDE for Windows and iOS/Linus.  
# For Window, uses EOS Studio Web which can be access using the following link
## https://www.eosstudio.io/
## https://app.eosstudio.io/
# For iOS/Linux, EOS Studio can be download and install into the machine directly, it also need Docker installed to run.  For more information, need to google it.
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