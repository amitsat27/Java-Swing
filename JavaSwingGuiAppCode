package Assignment5;
import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.sql.*;
import java.util.Scanner;


public class Assignment5 implements ActionListener {
    private static boolean dec = true,delete = false;
    private static boolean tablecreate = false;
    private int row=0;
    private JFrame f;
    private JLabel heading, roll, fullname, branch;
    private JTextField rollt, fullnamet, brancht;
    private JTextArea tabletext,utext;
    private JScrollPane tableview;
    private JButton addcon, show,Update,Delete,Drop;
    private JTable student;
    private JOptionPane showtext;
    static boolean id = true;
    public Assignment5() {
        DefaultTableModel model = new DefaultTableModel(new String[]{"ID","RollNo","FullName","Branch"},0);
        f = new JFrame("Student Database");
        f.addMouseListener(new MouseListener() {
            @Override
            public void mouseClicked(MouseEvent mouseEvent) {
                System.out.println(mouseEvent.getX() + " " + mouseEvent.getY());
            }

            @Override
            public void mousePressed(MouseEvent mouseEvent) {
                System.out.println(mouseEvent.getX() + " " + mouseEvent.getY());
            }

            @Override
            public void mouseReleased(MouseEvent mouseEvent) {
                System.out.println(mouseEvent.getX() + " " + mouseEvent.getY());
            }

            @Override
            public void mouseEntered(MouseEvent mouseEvent) {
                System.out.println(mouseEvent.getX() + " " + mouseEvent.getY());
            }

            @Override
            public void mouseExited(MouseEvent mouseEvent) {
                System.out.println(mouseEvent.getX() + " " + mouseEvent.getY());
            }
        });

        //TEXT ADDING
        heading = new JLabel("Student Database Using Swing");
        heading.setBounds(200, 10, 1000, 20);
        heading.setFont(new Font("Courier New", Font.BOLD, 20));
        heading.setForeground(Color.BLUE);
        roll = new JLabel("Enter Student Roll no  :");
        roll.setBounds(52, 120, 250, 30);
        rollt = new JTextField();
        rollt.setBounds(52, 150, 150, 20);
        fullname = new JLabel("Enter Student FullName   : ");
        fullnamet = new JTextField();
        fullname.setBounds(52, 180, 250, 30);
        fullnamet.setBounds(52, 210, 150, 20);
        branch = new JLabel("Enter Student Branch :");
        brancht = new JTextField();
        branch.setBounds(52, 230, 250, 30);
        brancht.setBounds(52, 260, 150, 20);


        //BUTTONS ADDING
        addcon = new JButton("Add  ");
        addcon.setBounds(52, 300, 150, 30);
        show = new JButton("Refresh Table");
        show.setBounds(52, 340, 150, 30);
        Update = new JButton("Update");
        Update.setBounds(52,380,150,30);
        Delete = new JButton("Delete");
        Delete.setBounds(52,420,150,30);

        student = new JTable();
        tableview = new JScrollPane(student);
        tableview.setHorizontalScrollBarPolicy(JScrollPane.HORIZONTAL_SCROLLBAR_ALWAYS);
        tableview.setVerticalScrollBarPolicy(JScrollPane.VERTICAL_SCROLLBAR_ALWAYS);
        tableview.setBounds(310, 120, 350, 450);
        f.add(tableview);

        //ACTION PERFORMED

        addcon.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent actionEvent) {
                try {
                    Scanner scanner = new Scanner(System.in);
                    Connection con = getconnection();
                    int rollk = Integer.parseInt(rollt.getText());
                    String full = fullnamet.getText();
                    String branch = brancht.getText();
                    if( tablecreate && (rollk == Integer.parseInt((String) model.getValueAt(row-1,1)))){
                        showtext.showMessageDialog(f,"Same Record Cannot be Added");
                    }else {
                        tablecreate = true;
                        PreparedStatement statement = con.prepareStatement("INSERT INTO student(Rollno,FullName,Branch) VALUES(?,?,?)");
                        statement.setInt(1, rollk);
                        statement.setString(2, full);
                        statement.setString(3, branch);

                        statement.executeUpdate();
                        showtext.showMessageDialog(f, "Record Added Successfully");
                    }
                } catch (Exception e) {
                    System.out.println(e);
                }
            }
        });
        show.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent actionEvent) {
                try{
                    Connection con = getconnection();
                    PreparedStatement statement1;
                    if(id) {
                        statement1 = con.prepareStatement("SELECT * FROM student ");
                        id = false;
                    }else{
                        statement1 = con.prepareStatement("SELECT * FROM student Where ID >=(SELECT MAX(ID) FROM student)");
                    }

                    ResultSet rs = statement1.executeQuery();
                    while(rs.next()){
                        int id = rs.getInt(1);
                        int r = rs.getInt(2);
                        String fn = rs.getString(3);
                        String bran = rs.getString(4);
                        if(id == row || delete ){
                            break;
                        }else{
                            model.addRow(new Object[]{String.valueOf(id),String.valueOf(r),fn,bran});
                        }
                        row++;
                    }
                    student.setModel(model);
                }catch (Exception e){
                    System.out.println(e);
                }
            }
        });
        Update.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent actionEvent)  throws  NullPointerException{
                try{
                    Connection con = getconnection();
                    String sroll = JOptionPane.showInputDialog(f,"Enter id :");
                    int id = Integer.parseInt(sroll);
                    PreparedStatement statement = con.prepareStatement("UPDATE student SET FullName = ?,Branch = ? where ID = ?");
                    String Fullname = JOptionPane.showInputDialog(f,"Enter Student fullName : ");
                    String Branch = JOptionPane.showInputDialog(f,"Enter Branch : ");
                    statement.setString(1,Fullname);
                    statement.setString(2,Branch);
                    statement.setInt(3,id);
                    statement.executeUpdate();
                    model.setValueAt(Fullname,id-1,2);
                    model.setValueAt(Branch,id-1,3);
                    student.setModel(model);

                }catch(Exception e){
                    System.out.println(e);
                }
            }
        });
        Delete.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent actionEvent) {
                try{
                    Connection con = getconnection();
                    String remove = JOptionPane.showInputDialog(f,"Enter the ID : ");
                    int rem = Integer.parseInt(remove);
                    PreparedStatement statement = con.prepareStatement("DELETE FROM student where ID = ?");
                    statement.setInt(1,rem);
                    statement.executeUpdate();
                    model.removeRow(rem-1);
                    delete = true;
                }catch (Exception e){
                    System.out.println(e);
                }
            }
        });

        //Adding JLabels and Jtextarea
        f.add(heading);
        f.add(roll);
        f.add(rollt);
        f.add(fullname);
        f.add(fullnamet);
        f.add(branch);
        f.add(brancht);

        //Adding Jbuttons
        f.add(show);
        f.add(addcon);
        f.add(Update);
        f.add(Delete);

        f.setSize(700, 700);
        f.setLayout(null);
        f.setVisible(true);

    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new Assignment5();

            }
        });


    }

    public static Connection getconnection() throws Exception, SQLException {
        try {
            String JbDriver = "com.mysql.cj.jdbc.Driver";
            String url = "jdbc:mysql://localhost:3306/mysql";
            String USER = "JDBC";
            String PASS = "root";
            Class.forName(JbDriver);
            Connection con = DriverManager.getConnection(url, USER, PASS);
            if (Assignment5.dec) {
                System.out.println("Connected To SQL Server");
            }
            Assignment5.dec = false;
            return con;
        } catch (Exception e) {
            System.out.println(e);
        }
        return null;

    }


    @Override
    public void actionPerformed(ActionEvent actionEvent) {



    }


}
