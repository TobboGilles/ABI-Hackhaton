pragma ton-solidity >= 0.35.0;


contract Asset {
  string public ProductName;
  address public custodianPubkey;
  STATUSES public status;

  enum STATUSES {
    CREATED,
    SENT,
    RECEIVED
  }


 constructor(string _ProductName) public {
    // Set ProductName
    ProductName = _ProductName;

    // Make deployer custodian
    msg.pubkey() == custodianPubkey;
    
        tvm.accept();


    // Update status to "CREATED"
    status = STATUSES.CREATED;
  }

  function send(address _to) public {
    // Must be custodian to send
    require(msg.pubkey() == custodianPubkey);
      
          // accept
         tvm.accept();

    // Can't be in "SENT" status
    // Must be "CREATED" or "RECEIVED"
    require(status != STATUSES.SENT);
     

         // accept
         tvm.accept();

    // Update status to "SENT"
    status = STATUSES.SENT;

    // Make _to new custodian
    custodianPubkey = _to;

  }

  function receive() public {
    // Must be custodian to receive
     require(msg.pubkey() == custodianPubkey);    
         
         // accept 
           tvm.accept();

    // Must be in "SENT" status
    // Cannot be "CREATED" or "RECEIVED"
    require(status == STATUSES.SENT);
          
        tvm.accept();
    
    // Update status to "RECEIVED"
       status = STATUSES.RECEIVED;
  }
}

