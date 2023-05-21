1.AddServerIntf.java
import java.rmi.*;                 //method declare
public interface AddServerIntf extends Remote {
double add(double d1, double d2) throws RemoteException;
}

2.AddServerImpl.java
import java.rmi.*;                                           
import java.rmi.server.*;
public class AddServerImpl extends UnicastRemoteObject
implements AddServerIntf {
public AddServerImpl() throws RemoteException {
}
public double add(double d1, double d2) throws RemoteException {
return d1 + d2;
}
}
3.AddServer.java
import java.net.*;
import java.rmi.*;
public class AddServer {
public static void main(String args[]) {
try {
AddServerImpl addServerImpl = new AddServerImpl(); //Servelimpl obj
Naming.rebind("AddServer", addServerImpl); //calling obj ..stub
}
catch(Exception e) {
System.out.println("Exception: " + e);
}
}
}

4.AddClient.java
import java.rmi.*;
public class AddClient {
public static void main(String args[]) {
try {
String addServerURL = "rmi://" + args[0] + "/AddServer";//server url location 
AddServerIntf addServerIntf =
(AddServerIntf)Naming.lookup(addServerURL);//interface method is called to check if there is url present..or obj is present or not..obj are not created in interface  so we check if there is any
System.out.println("The first number is: " + args[1]);
double d1 = Double.valueOf(args[1]).doubleValue();
System.out.println("The second number is: " + args[2]);
double d2 = Double.valueOf(args[2]).doubleValue();
System.out.println("The sum is: " + addServerIntf.add(d1, d2));
}
catch(Exception e) {
System.out.println("Exception: " + e);
}
}
}
Working:1. Open Terminal..ls
2.Compile: javac *.java
2. Generate stubs..... rmic AddServerImpl.... it will generate  AddServerImpl_Stub.class file.
3. Copy AddClient.class, AddServerImpl_Stub.class, and AddServerIntf.class to a directory on the client machine.
4.. Copy AddServerIntf.class, AddServerImpl.class, AddServerImpl_ Stub.class, and AddServer.class to a directory on the server machine.
5. Start the RMI Registry on the Server Machine using ..rmiregistry..
6. Start the Server using ..java AddServer.. in new terminal
7.Start the Client java AddClient servername/ip_address.... java AddClient 127.0.0.1 4 5
