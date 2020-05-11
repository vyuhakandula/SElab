# SElab



import java.awt.GridLayout;
import java.awt.Label;
import java.awt.List;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;
import java.sql.*;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JMenuItem;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextField;
import javax.swing.table.DefaultTableModel;

public class Medicine{
/**
*
*/
private static final long serialVersionUID = 1L;
private JButton insertButton,deleteButton,updateButton,viewButton;
private JPanel p1,p2,p3,p;
private JLabel lblmid,lblmname,lblprice,lblinstock;
private JTextField txtmid,txtmname,txtprice,txtinstock;

private List MIDList;
Connection con;ResultSet rs;
Statement statement;
private JFrame frame;
private JMenuItem insert,delete,update,view;
public Medicine(JPanel p,JFrame frame,JMenuItem insert,JMenuItem delete,JMenuItem update,JMenuItem view)
{

try
{
Class.forName("oracle.jdbc.driver.OracleDriver");
}

connectToDB();

this.frame=frame;
this.insert=insert;
this.delete=delete;
this.update=update;
this.view=view;

lblmid=new JLabel("Medicine ID");
lblmname=new JLabel("Medicine Name");
lblprice=new JLabel("Price");
lblinstock=new JLabel("In_Stock");


txtmid=new JTextField(15);
txtmname=new JTextField(15);
txtprice=new JTextField(8);
txtinstock=new JTextField(15);

this.p=p;



}

public void connectToDB()
 {

statement=con.createStatement();
statement.executeUpdate("commit");


}

 }
private void displaySQLErrors(SQLException e)
{
JOptionPane.showMessageDialog(p,"\nSQLException: " + e.getMessage() + "\n"+"SQLState: " + e.getSQLState() + "\n"+"VendorError: " + e.getErrorCode() + "\n");


}
public void loadMedIDs() {
try {
MIDList.removeAll();
rs=statement.executeQuery("select mid from medicine");
while(rs.next()) {
MIDList.add(rs.getString("mid"));
}
}
catch(SQLException e) {
displaySQLErrors(e);
}
}

public void buildGUI() {



insert.addActionListener(new ActionListener() {

@Override
public void actionPerformed(ActionEvent arg0) {
// TODO Auto-generated method stub
insertButton=new JButton("insert");
txtmid.setText(null);
txtmname.setText(null);
txtprice.setText(null);
txtinstock.setText(null);


p.removeAll();
frame.invalidate();
frame.validate();
frame.repaint();


p1=new JPanel();

p1.setLayout(new GridLayout(4,2));
p1.add(lblmid);
p1.add(txtmid);
p1.add(lblmname);
p1.add(txtmname);
p1.add(lblprice);
p1.add(txtprice);
p1.add(lblinstock);
p1.add(txtinstock);

p3=new JPanel(new FlowLayout());
p3.add(insertButton);
//p1.add(txtf1);
p3.setBackground(Color.yellow);
p1.setBounds(115,80,300,250);p3.setBounds(200,350,75,35);
p1.setBackground(Color.pink) ;





p2 = new JPanel(new FlowLayout());

MIDList=new List(10);
loadMedIDs();
p2.add(MIDList);p2.setBackground(Color.cyan) ;

p2.setBounds(450,150,350,180);


p. add(p1);p.add(p3);
p. add(p2);


p.setLayout(new BorderLayout());

frame.add(p);
frame.setSize(800,800);
frame.validate();

insertButton.addActionListener(new ActionListener() {
@Override
public void actionPerformed(ActionEvent e) {
// TODO Auto-generated method stub
try {
String query="INSERT INTO medicine VALUES("+txtmid.getText()+",'"+txtmname.getText()+"',"+txtprice.getText()+",'"+txtinstock.getText()+"')";

int i=statement.executeUpdate(query);
JOptionPane.showMessageDialog(p,"\ninserted "+i+" rows succesfully");loadMedIDs();



}
catch(SQLException insertException){
displaySQLErrors(insertException);
}

}


});
}
});
 }
