import java.util.*;

// Item class (NO main method here)
class Item {
    int id;
    String name;
    double price;
    int quantity;

    Item(int id, String name, double price, int quantity) {
        this.id = id;
        this.name = name;
        this.price = price;
        this.quantity = quantity;
    }
}

// Main class (Program starts here)
public class InventoryBillingSystem {

    static HashMap<Integer, Item> inventory = new HashMap<>();
    static Scanner sc = new Scanner(System.in);

    // Add Item
    public static void addItem() {
        System.out.print("Enter Item ID: ");
        int id = sc.nextInt();

        sc.nextLine(); // FIX for string input
        System.out.print("Enter Item Name: ");
        String name = sc.nextLine();

        System.out.print("Enter Price: ");
        double price = sc.nextDouble();

        System.out.print("Enter Quantity: ");
        int qty = sc.nextInt();

        inventory.put(id, new Item(id, name, price, qty));
        System.out.println("Item Added Successfully!\n");
    }

    // Update Stock
    public static void updateStock() {
        System.out.print("Enter Item ID to update: ");
        int id = sc.nextInt();

        if (inventory.containsKey(id)) {
            System.out.print("Enter New Quantity: ");
            int qty = sc.nextInt();
            inventory.get(id).quantity = qty;
            System.out.println("Stock Updated!\n");
        } else {
            System.out.println("Item Not Found!\n");
        }
    }

    // Display Items
    public static void displayItems() {
        System.out.println("\n--- Inventory List ---");
        if (inventory.isEmpty()) {
            System.out.println("No items available.");
        } else {
            for (Item item : inventory.values()) {
                System.out.println("ID: " + item.id +
                        " | Name: " + item.name +
                        " | Price: " + item.price +
                        " | Qty: " + item.quantity);
            }
        }
        System.out.println();
    }

    // Generate Bill
    public static void generateBill() {
        ArrayList<Item> billItems = new ArrayList<>();
        double total = 0;

        while (true) {
            System.out.print("Enter Item ID to purchase (0 to stop): ");
            int id = sc.nextInt();

            if (id == 0) break;

            if (inventory.containsKey(id)) {
                Item item = inventory.get(id);

                System.out.print("Enter Quantity: ");
                int qty = sc.nextInt();

                if (qty <= item.quantity) {
                    total += qty * item.price;
                    item.quantity -= qty;

                    billItems.add(new Item(item.id, item.name, item.price, qty));
                } else {
                    System.out.println("Not enough stock!\n");
                }
            } else {
                System.out.println("Item not found!\n");
            }
        }

        // Print Bill
        System.out.println("\n====== BILL ======");
        for (Item item : billItems) {
            System.out.println(item.name + " x " + item.quantity +
                    " = " + (item.price * item.quantity));
        }
        System.out.println("------------------");
        System.out.println("Total Amount: " + total);
        System.out.println("==================\n");
    }

    public static void main(String[] args) {

        while (true) {
            System.out.println("===== Inventory Billing System =====");
            System.out.println("1. Add Item");
            System.out.println("2. Update Stock");
            System.out.println("3. Display Items");
            System.out.println("4. Generate Bill");
            System.out.println("5. Exit");

            System.out.print("Enter choice: ");
            int choice = sc.nextInt();

            switch (choice) {
                case 1: addItem(); break;
                case 2: updateStock(); break;
                case 3: displayItems(); break;
                case 4: generateBill(); break;
                case 5:
                    System.out.println("Exiting...");
                    return;
                default:
                    System.out.println("Invalid Choice!\n");
            }
        }
    }
}
