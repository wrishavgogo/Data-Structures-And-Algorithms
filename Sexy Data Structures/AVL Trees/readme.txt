On Excali Draw i explained myself how left rotate and right rotate works 
As of now i only have a picture explaining insertion 
yet to do deletion 


I had a question bugging me in my mind , 

after insertion , for the path we chose for insertion 
  while backtracking we were checking balance factor for all the ancestors and trying to rebalance them 

  I was wondering why we need to do for all ancestors , while inserting if we find some disbalance we can just balance them and come back 

But then on excali Draw i drew an example where the insertion was happening deep down , but the root node got unbalanced , so hence we need to check all ancestors .



    Even incase of deletion , it will lead to issues as lets say a node got deleted and it made the root node unbalance , hence recursively checking on all ancestors during back tracking helps 
    keep the whole node balance maintained 
