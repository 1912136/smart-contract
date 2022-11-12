// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Hospital{

    mapping(uint => Dr) public dr;
    mapping(uint => Pat) private pat;
    mapping(uint => string[]) private disease;    
    mapping(uint => bool) public isAdmited;
    mapping(uint => bool) public isEmployed;
    
    
    struct Dr{
        string name;
        string qualification;
        string specialist;
        uint256 salary;
        
    }

    struct Pat{
        string name;
        uint8 age;
        uint256 admitDate;
        
    }

    address public admin;

    modifier onlyAdmin(){
        require(admin == msg.sender, "You are not the Admin");
        _;
    }
     
    constructor(){
       admin = msg.sender;
    }


    function admitPat(uint256 id, string memory name, uint8 age, string memory _disease) external onlyAdmin{
        require(isAdmited[id] == false, "Already Admited");
        pat[id] = Pat({name: name, age: age, admitDate: block.timestamp });
        disease[id].push(_disease);
        isAdmited[id] = true;
    }
 
    function addRecord(uint256 id, string memory _disease) external onlyAdmin{
        require(isAdmited[id] == true, "Patient is not Admited");
        disease[id].push(_disease);
    }
 
    function paientRecord(uint256 id) external view returns(Pat memory, string[] memory){
        return(pat[id], disease[id]);
    }
 
    function addDr(uint256 id, string memory name, string memory qualification, string memory specialist, uint256 salary) external onlyAdmin{
        require(isEmployed[id] == false, "Already Employed");
        dr[id] = Dr({name: name, qualification: qualification, specialist: specialist, salary: salary });
        isEmployed[id] = true;
    }


}