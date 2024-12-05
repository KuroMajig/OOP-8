# OOP-8
<h2>Latihan</h2>
<img width="622" alt="OOP-8" src="https://github.com/user-attachments/assets/3b688aa6-a5bf-4c18-ba4b-ae671c251c49">
<h2>Code</h2>
<h4>Customer Class</h4>

```java
class Customer {
    private final String name;
    private final String address;

    public Customer(String name, String address) {
        this.name = name;
        this.address = address;
    }

    public String getName() {
        return name;
    }

    public String getAddress() {
        return address;
    }
}
```
<h4>Item Class</h4>

```java
class Item {
    private final String shippingWeight;
    private final String description;
    private final float price;
    private final float taxRate;

    public Item(String shippingWeight, String description, float price, float taxRate) {
        this.shippingWeight = shippingWeight;
        this.description = description;
        this.price = price;
        this.taxRate = taxRate;
    }

    public float getPriceForQuantity(int quantity) {
        return price * quantity;
    }

    public float getTax() {
        return price * taxRate;
    }

    public boolean inStock() {
        return true;
    }

    public String getShippingWeight() {
        return shippingWeight;
    }

    public String getDescription() {
        return description;
    }
}
```
<h4>Order Class</h4>

```java
class Order {
    private final String date;
    private final String status;
    private final Customer customer;
    private final OrderDetail[] orderDetails;

    public Order(String date, String status, Customer customer, OrderDetail[] orderDetails) {
        this.date = date;
        this.status = status;
        this.customer = customer;
        this.orderDetails = orderDetails;
    }

    public float calcSubTotal() {
        float subTotal = 0;
        for (OrderDetail detail : orderDetails) {
            subTotal += detail.calcSubTotal();
        }
        return subTotal;
    }

    public float calcTax() {
        float tax = 0;
        for (OrderDetail detail : orderDetails) {
            tax += detail.calcTax();
        }
        return tax;
    }

    public float calcTotal() {
        return calcSubTotal() + calcTax();
    }

    public float calcTotalWeight() {
        float totalWeight = 0;
        for (OrderDetail detail : orderDetails) {
            totalWeight += detail.calcWeight();
        }
        return totalWeight;
    }

    public String getDate() {
        return date;
    }

    public String getStatus() {
        return status;
    }

    public Customer getCustomer() {
        return customer;
    }

    public OrderDetail[] getOrderDetails() {
        return orderDetails;
    }
}
```
<h4>Orderdetail Class</h4>

```java
class OrderDetail {
    private final int quantity;
    private final String taxStatus;
    private final Item item;

    public OrderDetail(int quantity, String taxStatus, Item item) {
        this.quantity = quantity;
        this.taxStatus = taxStatus;
        this.item = item;
    }

    public float calcSubTotal() {
        return item.getPriceForQuantity(quantity);
    }

    public float calcWeight() {
        return Float.parseFloat(item.getShippingWeight()) * quantity;
    }

    public float calcTax() {
        return item.getTax();
    }

    public int getQuantity() {
        return quantity;
    }

    public String getTaxStatus() {
        return taxStatus;
    }

    public Item getItem() {
        return item;
    }
}
```
<h4>Payment (Abstract Class)</h4>

```java
abstract class Payment {
    protected float amount;

    public Payment(float amount) {
        this.amount = amount;
    }

    public float getAmount() {
        return amount;
    }
}
```
<h4>Main Class</h4>

```java
public class main {
    public static void main(String[] args) {
        Item item1 = new Item("1.5", "Keyboard", 200000.0f, 0.05f);
        Item item2 = new Item("0.5", "Mouse", 100000.0f, 0.05f);
        OrderDetail detail1 = new OrderDetail(2, "Taxable", item1);
        OrderDetail detail2 = new OrderDetail(3, "Taxable", item2);

        Customer customer = new Customer("Kuro", "Japan, Tokyo");

        Order order = new Order("2024-12-04", "Processing", customer, new OrderDetail[]{detail1, detail2});

        System.out.println("Customer: " + order.getCustomer().getName());
        System.out.println("Order Date: " + order.getDate());
        System.out.println("Order Status: " + order.getStatus());

        System.out.println();

        for (OrderDetail detail : order.getOrderDetails()) {
            Item item = detail.getItem();
            System.out.println("Item: " + item.getDescription());
            System.out.println("Quantity: " + detail.getQuantity());
            System.out.println("Price per item: " + String.format("%.0f", item.getPriceForQuantity(1)));
            System.out.println("SubTotal: " + String.format("%.0f", detail.calcSubTotal())); 
        }

        System.out.println();

        System.out.println("Order SubTotal: " + String.format("%.0f", order.calcSubTotal())); 
        System.out.println("Order Tax: " + String.format("%.0f", order.calcTax()));
        System.out.println("Order Total: " + String.format("%.0f", order.calcTotal())); 
        System.out.println("Total Weight: " + String.format("%.2f", order.calcTotalWeight()) + " lbs"); 
    }
}
```
<h4>Output</h4>

```
Customer: Kuro
Order Date: 2024-12-05
Order Status: Processing

Item: Keyboard
Quantity: 2
Price per item: 200000
SubTotal: 400000
Item: Mouse
Quantity: 3
Price per item: 100000
SubTotal: 300000

Order SubTotal: 700000
Order Tax: 15000
Order Total: 715000
Total Weight: 4.50 lbs
```
