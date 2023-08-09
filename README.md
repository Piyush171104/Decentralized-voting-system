pragma solidity ^0.8.0;

contract VotingSystem {
    // Structure to represent a candidate
    struct Candidate {
        uint id;
        string name;
        uint voteCount;
    }
    
    // Mapping of candidate IDs to Candidate objects
    mapping(uint => Candidate) public candidates;
    
    // Mapping of voter addresses to their vote status
    mapping(address => bool) public hasVoted;
    
    // Total number of candidates
    uint public totalCandidates;
    
    // Constructor to initialize candidates
    constructor(string[] memory _candidateNames) {
        for (uint i = 0; i < _candidateNames.length; i++) {
            totalCandidates++;
            candidates[totalCandidates] = Candidate(totalCandidates, _candidateNames[i], 0);
        }
    }
    
    // Function to cast a vote for a candidate
    function vote(uint _candidateId) public {
        require(!hasVoted[msg.sender], "You have already voted.");
        require(_candidateId > 0 && _candidateId <= totalCandidates, "Invalid candidate ID.");
        
        candidates[_candidateId].voteCount++;
        hasVoted[msg.sender] = true;
    }
    
    // Function to get the vote count for a candidate
    function getVoteCount(uint _candidateId) public view returns (uint) {
        require(_candidateId > 0 && _candidateId <= totalCandidates, "Invalid candidate ID.");
        return candidates[_candidateId].voteCount;
    }
}