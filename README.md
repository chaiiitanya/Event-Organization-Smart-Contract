Blockchain Project: Event Organisation using Smart contract

The way this smart contract works is the Event Planner and the user/ticket buyer both can work.
The user with an account can create the events by entering details like Event name, time, ticket
price and amount.
The User can buy the tickets by entering the event id and the amount of tickets the user want to
buy, the user must have enough eth or virtual 100 wei to buy the tickets.
These tickets can be transferred from one account to other using their id and amount they want
to tranfer.
All of the processes are performed on Remix IDE working on the Google Chrome Browser using
Internet.
The Smart contract is first developed on Remix IDE, then we have to compile it and deploy it.
In deployed the contracts, the above functions can be easily performed by both parties.

TechStack used: Chrome Browser, Remix IDE for Smart contract, Metamask for test ether wallet, Ganache for 100 Ether Account

Smart Contract:

//SPDX-License-Identifier: Unlicense
pragma solidity >=0.5.0 <0.9.0;


contract EventContract {
 struct Event{
   address organizer;
   string name;
   uint date; //0 1 2
   uint price;
   uint ticketCount;  //1 sec  0.5 sec
   uint ticketRemain;
 }


 mapping(uint=>Event) public events;
 mapping(address=>mapping(uint=>uint)) public tickets;
 uint public nextId;
 


 function createEvent(string memory name,uint date,uint price,uint ticketCount) external{
   require(date>block.timestamp,"You can organize event for future date");
   require(ticketCount>0,"You can organize event only if you create more than 0 tickets");


   events[nextId] = Event(msg.sender,name,date,price,ticketCount,ticketCount);
   nextId++;
 }


 function buyTicket(uint id,uint quantity) external payable{
   require(events[id].date!=0,"Event does not exist");
   require(events[id].date>block.timestamp,"Event has already occured");
   Event storage _event = events[id];
   require(msg.value==(_event.price*quantity),"Ethere is not enough");
   require(_event.ticketRemain>=quantity,"Not enough tickets");
   _event.ticketRemain-=quantity;
   tickets[msg.sender][id]+=quantity;


 }

 function transferTicket(uint id, uint quantity,address to) public{
    require(events[id].date!=0,"Event Does Not Exist");
    require(events[id].date>block.timestamp,"Event Has Ended");
    require(tickets[msg.sender][id]>=quantity,"You do not have new tickets to transfer");
    tickets[msg.sender][id]-=quantity;
    tickets[to][id]+=quantity;
 }
}
