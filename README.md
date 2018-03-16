pragma solidity ^0.4.18;
   
  contract ERC20Interface {
   
      function totalSupply() constant returns (uint256 totalSupply);
   
   
      function balanceOf(address _owner) constant returns (uint256 balance);
   
   
      function transfer(address _to, uint256 _value) returns (bool success);
   
   
      function transferFrom(address _from, address _to, uint256 _value) returns (bool success);
   
      function approve(address _spender, uint256 _value) returns (bool success);
   
      function allowance(address _owner, address _spender) constant returns (uint256 remaining);
   
      event Transfer(address indexed _from, address indexed _to, uint256 _value);
   
      event Approval(address indexed _owner, address indexed _spender, uint256 _value);
  }
   
 contract TestToken is ERC20Interface {
      string public constant symbol = "TEST";
      string public constant name = "TEST Token";
      uint8 public constant decimals = 8;
      uint256 _totalSupply = 6300000000000000;
     
      address public owner;
   
      // Balances for each account
      mapping(address => uint256) balances;
   
      // Owner of account approves the transfer of an amount to another account
      mapping(address => mapping (address => uint256)) allowed;
   
      // Functions with this modifier can only be executed by the owner
      modifier onlyOwner() {
          if (msg.sender != owner) {
             throw;
          }
          _;
      }

      modifier notPaused {
        require(now > 1521133200  || msg.sender == owner);
        _;
      }
      
      function TestToken() {
        owner = msg.sender;
        balances[owner] = _totalSupply;
      }

      function totalSupply() constant returns (uint256 totalSupply) {
          totalSupply = _totalSupply;
      }
   
      // What is the balance of a particular account?
      function balanceOf(address _owner) constant returns (uint256 balance) {
          return balances[_owner];
      }
   
      // Transfer the balance from owner's account to another account
      function transfer(address _to, uint256 _amount) public notPaused returns (bool success) {
          if (balances[msg.sender] >= _amount && _amount > 0 && balances[_to] + _amount > balances[_to]) {
              balances[msg.sender] -= _amount;
              balances[_to] += _amount;
              Transfer(msg.sender, _to, _amount);
              return true;
          } else {
              return false;
          }
      }
  
      function transferFrom(
          address _from,
          address _to,
          uint256 _amount
      ) returns (bool success) {
         if (balances[_from] >= _amount
             && allowed[_from][msg.sender] >= _amount
             && _amount > 0
             && balances[_to] + _amount > balances[_to]) {
             balances[_from] -= _amount;
             allowed[_from][msg.sender] -= _amount;
             balances[_to] += _amount;
             Transfer(_from, _to, _amount);
             return true;
         } else {
             return false;
         }
      }
  
      // Allow _spender to withdraw from your account, multiple times, up to the _value amount.
      // If this function is called again it overwrites the current allowance with _value.
      function approve(address _spender, uint256 _amount) returns (bool success) {
         allowed[msg.sender][_spender] = _amount;
         Approval(msg.sender, _spender, _amount);
         return true;
      }
  
      function allowance(address _owner, address _spender) constant returns (uint256 remaining) {
         return allowed[_owner][_spender];
      }
 }
