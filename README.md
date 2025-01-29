// SPDX-License-Identifier: MIT
pragma solidity >=0.6.12 <0.9.0;
 
/**
 * @title VotingSystem
 * @dev A simple voting system that allows users to propose and vote on different options.
 */
contract VotingSystem {
    
    struct Proposal {
        string description;
        uint voteCount;
    }
 
    address public chairperson;
    mapping(address => bool) public voters;
    Proposal[] public proposals;
 
    /**
     * @dev Sets the chairperson and initializes the contract with a default proposal.
     * @param initialProposal The description of the initial proposal.
     */
    constructor(string memory initialProposal) {
        chairperson = msg.sender;
        addProposal(initialProposal);
    }
 
    /**
     * @dev Adds a new proposal. Only the chairperson can add proposals.
     * @param proposalDescription The description of the proposal.
     */
    function addProposal(string memory proposalDescription) public {
        require(msg.sender == chairperson, "Only the chairperson can add proposals.");
        proposals.push(Proposal({
            description: proposalDescription,
            voteCount: 0
        }));
    }
 
    /**
     * @dev Allows an address to be registered as a voter.
     * @param voter The address of the voter.
     */
    function registerVoter(address voter) public {
        require(msg.sender == chairperson, "Only the chairperson can register voters.");
        voters[voter] = true;
    }
 
    /**
     * @dev Casts a vote to a proposal. Only registered voters can vote.
     * @param proposalIndex The index of the proposal in the proposals array.
     */
    function vote(uint proposalIndex) public {
        require(voters[msg.sender], "Only registered voters can vote.");
        require(proposalIndex < proposals.length, "Invalid proposal index.");
        proposals[proposalIndex].voteCount += 1;
    }
 
    /**
     * @dev Returns the description and vote count of a proposal.
     * @param proposalIndex The index of the proposal in the proposals array.
     * @return description The description of the proposal.
     * @return voteCount The number of votes the proposal has received.
     */
    function getProposal(uint proposalIndex) public view returns (string memory description, uint voteCount) {
        require(proposalIndex < proposals.length, "Invalid proposal index.");
        Proposal storage proposal = proposals[proposalIndex];
        return (proposal.description, proposal.voteCount);
    }
}
