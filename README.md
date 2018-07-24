# TME3
for athabasca university course, COMP 308

This TME aims to create an implementation of a basic design called controller. You are going to modify these programs to show you the various ways you can build on the basic design model using the capabilities of Java. The TME is divided into four steps to help you organize your programming efforts. The intermediate stages serve as checkpoints; please submit only the final program. This program is further developed in TME4.
Step 1: Preparation and familiarization

1. Find a working directory for this TME. Download this zip file tme3Start.zip and unzip all files. The zip will create a subdirectory called tme3.

2. In tme3, compile all source java files.

3. In your working directory, compile GreenhouseControls.java. If there are any compilation errors, remove / reset your classpath and retry again.

4. Try running GreenhouseControls by:

java GreenhouseControls -f defaultEvents
5. Study the codes to examine how it functions.

Step 2: In this part, we add implementation to existing design

1. Add a boolean instance variable to GreenhouseControls called fans. Create two Event classes called FansOn and FansOff. The action() of these two classes should modify fan to true or false respectively.

2. Modify Bell Event so that it will be able to run an arbitrary number of times separated by 2000 msec each. To facilitate this requirement, you should generate the number of Bell Events specified in the rings parameter in the examples file. Please pay special attention to the possibility that other events might be run in between the various Bell events.

3. Modify Restart.action() to start the the system by reading events from a text file. Use Scanner and an appropriate regular expression.

4. Try running GreenhouseControls by:

java GreenhouseControls -f examples1.txtu
and

java GreenhouseControls -f examples2.txt

5. The -f argument must be present. It must be either -f or -d. Please see Part 4. The event information must be in the format specified in examples1.txt and examples2.txt.

Step 3: Add functionality to simulate problems

1. Create a WindowMalfunction and PowerOut Events to simulate problems that may occur in a GreenhouseControls. The event should set the following boolean variables as appropriate in GreenhouseControls:

windowok = false;
poweron = false;

After setting the variables, WindowMalfunction or PowerOut should throw an exception specifying the faulty condition. Create a ControllerException class that extends Exception for this purpose.

2. If an exception is thrown from WindowMalfunction or PowerOut, the Controller catches the exception, then initiates an emergency shutdown with an appropriate message. Add a method to Controller called shutdown, and override this method in GreenhouseControls to accomplish the shutdown.

3. Add an instance variable in GreenhouseControls called errorcode. It indicates the nature of the problem with an error code in an int variable errorcode (1 for WindowMalfunction and 2 for PowerOut), logs the time and the reason for the shutdown in a text file in the current directory called error.log and prints it to the console. It then serializes and saves the entire GreenhouseControls object in a file dump.out in the current directory before exiting.

4. Run the following to test this part:

java GreenhouseControls f examples3.txt
and

java GreenhouseControls -f examples4.txt

Step 4: System restore

In this part, we add functionality for restoring the saved GreenhouseControls object and having it resume execution where it left off. It demonstrates the use of interfaces and the capability of Java methods to return objects.

1. Create the following interface

interface Fixable {
// turns Power on, fix window and zeros out error codes

void fix ();

// logs to a text file in the current directory called fix.log

// prints to the console, and identify time and nature of

// the fix

void log();

}

2. Create inner classes PowerOn and FixWindow that implement Fixable.

3. Add a method to GreenhouseControls

int getError();
which returns the error code saved.

4. Add a method within GreenhouseControls

Fixable getFixable(int errorcode);
which returns the appropriate Fixable object to correct the error and reset the error code to zero.

5. Create a class Restore to restore the system after a shutdown. When this command line is called:

java GreenhouseControls -d dump.out
a Restore object should be created and the file name dump.out is passed to the Restore object. It then retrieves the GreenhouseControls object saved by any emergency shutdown. It has the state of the saved system printed to the console. It sets any variable that was changed by the Event that initiated the shutdown back to its normal state (e.g., poweron=true) using the appropriate Fixable. It then starts the system running at the event following the event that caused the shutdown. A proper error message should be displayed on the console if any exception or error is caught by the Restore object.
