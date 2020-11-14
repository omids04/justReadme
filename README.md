
//just a simple contract to explain. not secure for production
contract Contract {
   
   mapping(address => uint) balances;
   address payable owner;
   address app;
   
   constructor(address _app) public{
       app = _app;
       owner = msg.sender;
   }
   
   modifier onlyApp{
       require(msg.sender == app);
       _;
   }
   
   //if person A wants to sell his ethers, he sends his ethers to this contract using deposit function
   function deposit() public payable{
       balances[msg.sender] += msg.value;
   }
   // now if persan B wants to buy ether from person A he make a payment using his credit card
   // after payment payment completed, app automatically calls this function to transfer ether to person B account
   // assumption is that app's private key is kept secure in app 
   function transfer(address _from, address _to, uint _value) public onlyApp{
       require(balances[_from] >= _value);
       balances[_from] -= _value;
       balances[_to] += _value;
   }
   // now person B can withdraw his ethers from contract
   function withraw(uint _value) public payable{
       require(balances[msg.sender] >= _value);
       balances[msg.sender] -= _value;
       msg.sender.transfer(_value);
   }
   
}
