# CertiScore
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f717c58f-4e4e-40d3-b9d6-85aeef018814" />

# 🎓 Immutable Grade Ledger for Verified Credentials

> A blockchain-powered system for **transparent, tamper-proof academic credential storage** — built on Solidity and deployed on the **Celo Sepolia Testnet**.

---

## 🚀 Project Description

**Immutable Grade Ledger** is a decentralized smart contract that allows educational institutions to **record, verify, and publish student grades** in an immutable and transparent way.

Once a grade is entered, it **cannot be altered or deleted**, ensuring complete trust and authenticity in academic records.  
This project is ideal for learning **Solidity**, **smart contract deployment**, and **blockchain-based credential verification**.

---

## 💡 What It Does

- Lets an **authorized admin (institution)** add student grades to the blockchain.  
- Grades are **publicly viewable** but **not modifiable** by anyone.  
- Each record includes a student's name, course name, and marks.  
- Provides a **transparent ledger** of verified academic results.

---

## ✨ Features

✅ **Immutable Records** – Once a grade is added, it can never be changed or removed.  
✅ **Admin-Only Access** – Only the contract deployer (institution) can add grades.  
✅ **Public Verification** – Anyone can view a student’s grade using their unique ID.  
✅ **Lightweight and Gas Efficient** – Uses minimal data types for cheaper transactions.  
✅ **Event Logging** – Every grade addition is logged for transparency and easy tracking.

---

## 🧩 Smart Contract Code

Replace `//paste your code` below with your Solidity code.

```solidity
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
