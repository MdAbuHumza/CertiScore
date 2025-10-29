// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

/// @title Immutable Grade Ledger for Verified Credentials
/// @author Humza
/// @notice Stores student grades permanently on the blockchain
contract GradeLedger {

    // Address of the admin (e.g., institution)
    address public admin;

    // Struct to store grade info
    struct Grade {
        string studentName;
        string courseName;
        uint8 marks;   // use uint8 (0–100) for grades
        bool exists;   // to prevent duplicates
    }

    // Mapping: student ID → Grade
    mapping(string => Grade) private grades;

    // Event (helps in tracking changes via logs)
    event GradeAdded(string indexed studentId, string studentName, string courseName, uint8 marks);

    // Modifier to restrict access
    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can perform this action");
        _;
    }

    constructor() {
        admin = msg.sender; // whoever deploys contract becomes admin
    }

    /// @notice Add a student's grade (immutable once added)
    function addGrade(string memory studentId, string memory studentName, string memory courseName, uint8 marks)
        public
        onlyAdmin
    {
        require(!grades[studentId].exists, "Grade already exists for this student");
        require(marks <= 100, "Marks must be between 0 and 100");

        grades[studentId] = Grade(studentName, courseName, marks, true);

        emit GradeAdded(studentId, studentName, courseName, marks);
    }

    /// @notice View a student's grade by ID
    function getGrade(string memory studentId)
        public
        view
        returns (string memory, string memory, uint8)
    {
        require(grades[studentId].exists, "Grade not found");
        Grade memory g = grades[studentId];
        return (g.studentName, g.courseName, g.marks);
    }
}
