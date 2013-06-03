<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. What is CheckLT?</a></li>
<li><a href="#sec-2">2. Install</a></li>
<li><a href="#sec-3">3. Poor Man's Information Flow in Two Steps</a>
<ul>
<li><a href="#sec-3-1">3.1. Configuring The Lattice</a></li>
<li><a href="#sec-3-2">3.2. Label Syntax</a></li>
</ul>
</li>
<li><a href="#sec-4">4. Example: Protecting Internal Emails</a></li>
<li><a href="#sec-5">5. Acknowledgements and Thanks</a></li>
<li><a href="#sec-6">6. Related Works, People, Papers, Etc</a></li>
</ul>
</div>
</div>



# What is CheckLT?

CheckLT is a program verification tool for Java which can help you use taint tracking to find defects in your software. CheckLT provides an easy to install verification toolset, a simple, non-invasive syntax for annotating programs, and a dynamically configurable security lattice. 

CheckLT was born out of a research project at the University of Central Florida where we are researching creating tools for secure information flow analysis for Android applications. CheckLT does not implement full information flow security, but instead aims at being a lightweight security analysis toolkit for Java. 

Though CheckLT relies on Checker Framework for its base functionality (in fact, it is a plugin for the checker framework) CheckLT is being distributed independently of Checker Framework as a part of an experiment by this author to make formal verification tools more easily accessible outside of the research community. You can think of it as a "Click, Run, Verify!" approach to formal methods software!

# Install

Before you install CheckLT, make sure you have a recent Java 7 (1.7) installed. You can use either the Oracle JDK or OpenJDK.

To install, download the archive, and run the included jar file by double clicking on it. If for some reason your system does not suport running executable jar files by double clicking, you can install it from a command prompt by running:

    
    # java -jar checklt-installer.jar

After you install CheckLT, you can run it from the command prompt by running:

    
    # checklt MyProgram.java

Or, to run it on a whole source tree:

    
    # checklt -source-dir src


# Poor Man's Information Flow in Two Steps

To configure CheckLT for your application you must do two things. First, you must configure security rules for your particular application. In this process you will configure a *security lattice*, as we shall see in the next section. Once your security lattice is set up you can then annotate your program to reflect your concerns. The next two sections cover this process in detail.

## Configuring The Lattice

The first thing you must do is configure a security lattice for use with CheckLT. This file should be called `security.xml`, and it should be in the root directory from which you execute CheckLT. Note that CheckLT can (and will) check your proposed lattice for soundness before executing. As of the current version CheckLT checks to make sure that no insecure flows are specified (via graph cycle checking) and that all specified trusts are reachable and properly declared. 

The following example represents a slightly more complicated lattice than you will need for most projects. It is presented simply to showcase some of the flexibility of CheckLT.

    
    <linear-lattice>
    
        <!-- all levels must first be declared -->
        <levels>
            <level>Private</level>
            <level>UserTrusts</level>
            <level>User</level>
            <level>Public</level>
        </levels>
    
    
        <level-specs>
    
            <!-- level spec for Public -->
            <level-spec>
                <name>Public</name>
    
                <trusts>
                    <level>Private</level>
                    <level>UserTrusts</level>
                    <level>User</level>
                </trusts>
            </level-spec>
    
            <!-- level spec for User -->
            <level-spec>
                <name>User</name>
    
                <trusts>
                    <level>UserTrusts</level>
    
                </trusts>
            </level-spec>
    
    
            <!-- level spec for UserTrusts -->
            <level-spec>
                <name>UserTrusts</name>
    
                <trusts>
                    <level>Private</level>
    
                </trusts>
            </level-spec>
    
    
            <!-- level spec for UserTrusts -->
            <level-spec>
                <name>Private</name>
    
                <trusts>
                    <!-- trusts no one (other than self) -->
                </trusts>
            </level-spec>
    
        </level-specs>
    
    </linear-lattice>

While more complex lattice structures are possible to represent in CheckLT, as of the current release, CheckLT recognizes a linear lattice at the moment. 

Graphically represented, the lattice described above looks like the following figure

## Label Syntax

Labels in CheckLT are spe

# Example: Protecting Internal Emails

Pretend that you are tasked with the problem of creating an internal email system at a large corporation with lots of secrets and enemies. In light of this, the CEO has asked you to provide some assurance to him that it will not be possible for  

# Acknowledgements and Thanks

CheckLT is a direct byproduct of my work with Gary Leavens at the University of Central Florida and David Naumann the Stevens Insitute of Technology, to whom I am deeply grateful. Without their constant guideance (and correction!) and encouragement CheckLT wouldn't have been possible!

# Related Works, People, Papers, Etc

-   Gary Leaven's Research Page @ UCF

-   The Checker Framework

-   CGR

-   Lattice Flow Tainting
