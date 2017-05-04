[Mediator pattern](https://en.wikipedia.org/wiki/Mediator_pattern)
---------------

The **mediator pattern** defines an object that encapsulates how a set of objects interact. 

This pattern is considered to be a [behavioral pattern](https://en.wikipedia.org/wiki/Behavioral_pattern) due to the way it can alter the program's running behavior.

With **mediator pattern**, communication between objects is encapsulated within a **mediator** object. 

Objects no longer communicate directly with each other, but instead communicate through the mediator. This reduces the dependencies between communicating objects, thereby reducing [coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming)).

It promotes loose coupling by keeping objects from referring to each other explicitly, and it allows their interaction to be varied independently.

Client classes can use the mediator to send messages to other clients, and can receive messages from other clients via an event on the mediator class.


> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e4/Mediator_design_pattern.png/550px-Mediator_design_pattern.png" alt="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e4/Mediator_design_pattern.png/550px-Mediator_design_pattern.png" width="550" height="167" />


**Mediator** - defines the interface for communication between *Colleague* objects

**ConcreteMediator** - implements the Mediator interface and coordinates communication between *Colleague* objects. It is aware of all of the *Colleagues* and their purposes with regards to inter-communication.

**Colleague** - defines the interface for communication with other *Colleagues*

**ConcreteColleague** - implements the Colleague interface and communicates with other *Colleagues* through its *Mediator.*

In the following example a mediator object controls the status of three collaborating buttons: for this it contains three methods (book(), view() and search()) that set the status of the buttons.

The methods are called by each button upon activation (via the execute() method in each of them).

Hence here the collaboration pattern is that each participant (here the buttons) communicates to the mediator its activity and the mediator dispatches the expected behavior to the other participants.

```java
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;

//Colleague interface
interface Command {
    void execute();
}

//Abstract Mediator
interface Mediator {
    void book();
    void view();
    void search();
    void registerView(BtnView v);
    void registerSearch(BtnSearch s);
    void registerBook(BtnBook b);
    void registerDisplay(LblDisplay d);
}

//Concrete mediator
class ParticipantMediator implements Mediator {

    BtnView btnView;
    BtnSearch btnSearch;
    BtnBook btnBook;
    LblDisplay show;

    //....
    public void registerView(BtnView v) {
        btnView = v;
    }

    public void registerSearch(BtnSearch s) {
        btnSearch = s;
    }

    public void registerBook(BtnBook b) {
        btnBook = b;
    }

    public void registerDisplay(LblDisplay d) {
        show = d;
    }

    public void book() {
        btnBook.setEnabled(false);
        btnView.setEnabled(true);
        btnSearch.setEnabled(true);
        show.setText("booking...");
    }

    public void view() {
        btnView.setEnabled(false);
        btnSearch.setEnabled(true);
        btnBook.setEnabled(true);
        show.setText("viewing...");
    }

    public void search() {
        btnSearch.setEnabled(false);
        btnView.setEnabled(true);
        btnBook.setEnabled(true);
        show.setText("searching...");
    }
    
}

//A concrete colleague
class BtnView extends JButton implements Command {

    Mediator med;

    BtnView(ActionListener al, Mediator m) {
        super("View");
        addActionListener(al);
        med = m;
        med.registerView(this);
    }

    public void execute() {
        med.view();
    }
    
}

//A concrete colleague
class BtnSearch extends JButton implements Command {

    Mediator med;

    BtnSearch(ActionListener al, Mediator m) {
        super("Search");
        addActionListener(al);
        med = m;
        med.registerSearch(this);
    }

    public void execute() {
        med.search();
    }
    
}

//A concrete colleague
class BtnBook extends JButton implements Command {

    Mediator med;

    BtnBook(ActionListener al, Mediator m) {
        super("Book");
        addActionListener(al);
        med = m;
        med.registerBook(this);
    }

    public void execute() {
        med.book();
    }

}

class LblDisplay extends JLabel {

    Mediator med;

    LblDisplay(Mediator m) {
        super("Just start...");
        med = m;
        med.registerDisplay(this);
        setFont(new Font("Arial", Font.BOLD, 24));
    }

}

class MediatorDemo extends JFrame implements ActionListener {

    Mediator med = new ParticipantMediator();

    MediatorDemo() {
        JPanel p = new JPanel();
        p.add(new BtnView(this, med));
        p.add(new BtnBook(this, med));
        p.add(new BtnSearch(this, med));
        getContentPane().add(new LblDisplay(med), "North");
        getContentPane().add(p, "South");
        setSize(400, 200);
        setVisible(true);
    }

    public void actionPerformed(ActionEvent ae) {
        Command comd = (Command) ae.getSource();
        comd.execute();
    }

    public static void main(String[] args) {
        new MediatorDemo();
    }
}
```