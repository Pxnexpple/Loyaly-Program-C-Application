    //This method contains the media type
    enum MediaType
    {
        VHS,
        DVD,
        CD
    }

    //This method processes the menu option
    enum MainMenuOption
    {
        CalculateDiscount,
        CalculateFreeRentals,
        CalculateLostMediaRewards,
        OrderItems,
        Checkout,
        Exit
    }

    //This method stores the customers details
    class Customer
    {
        public string Name { get; set; }
        public int RegistrationYear { get; set; }
        public int CurrentYear { get; set; }
        public int TotalRentals { get; set; }
    }
    class OrderItem
    {
        public MediaType MediaType { get; set; }
        public string Title { get; set; }
        public double Price { get; set; }
    }
    internal class Program
    {
        //This method process the main function of the program to input customer details
        static void Main(string[] args)
        {
            Customer customer = GetCustomerDetails();
            List<OrderItem> orderItems = new List<OrderItem>();
            double discount = 0;
            int freeRentals = 0;
            List<string> lostMediaRewards = new List<string>();

            while (true)
            {
                MainMenuOption selectedOption = DisplayMainMenu();
                switch (selectedOption)
                {
                    case MainMenuOption.CalculateDiscount:
                        discount = CalculateDiscount(customer.RegistrationYear, customer.CurrentYear);
                        Console.WriteLine("Customer Discount: " + discount + "%");

                        break;

                    case MainMenuOption.CalculateFreeRentals:
                        freeRentals = CalculateFreeRentals(customer.TotalRentals);
                        Console.WriteLine("Free Rentals: " + freeRentals);

                        break;

                    case MainMenuOption.CalculateLostMediaRewards:
                        lostMediaRewards = CalculateLostMediaRewards(customer.RegistrationYear, customer.CurrentYear, customer.TotalRentals);
                        Console.WriteLine("Lost Media Rewards: " + string.Join(", ", lostMediaRewards));

                        break;

                    case MainMenuOption.OrderItems:
                        OrderItems(orderItems);

                        break;

                    case MainMenuOption.Checkout:
                        Checkout(orderItems, discount, freeRentals);

                        return;

                    case MainMenuOption.Exit:

                        return;
                }
            }
        }

        //This method allows the user to enter customer details
        static Customer GetCustomerDetails()
        {
            Customer customer = new Customer();
            Console.Write("Enter your name: ");
            customer.Name = Console.ReadLine();

            Console.Write("Enter the registration year: ");
            customer.RegistrationYear = int.Parse(Console.ReadLine());

            Console.Write("Enter the current year: ");
            customer.CurrentYear = int.Parse(Console.ReadLine());

            Console.Write("Enter the total number of rentals made: ");
            customer.TotalRentals = int.Parse(Console.ReadLine());

            return customer;
        }

        //This method displays the main menu
        static MainMenuOption DisplayMainMenu()
        {
            Console.WriteLine("Main Menu:");
            Console.WriteLine("1. Calculate Discount");
            Console.WriteLine("2. Calculate Free Rentals");
            Console.WriteLine("3. Calculate Lost Media Rewards");
            Console.WriteLine("4. Order Items");
            Console.WriteLine("5. Checkout");
            Console.WriteLine("6. Exit");
            Console.Write("Select an option: ");
            int choice = int.Parse(Console.ReadLine());
            return (MainMenuOption)(choice - 1);
        }

        //This method calculates the discount
        static double CalculateDiscount(int registrationYear, int currentYear)
        {
            int customerYears = currentYear - registrationYear;

            if (customerYears <= 4)
                return 5;
            else if (customerYears >= 5 && customerYears <= 9)
                return 10;
            else if (customerYears >= 10 && customerYears <= 14)
                return 20;
            else if (customerYears >= 15)
                return 35;
            else
                return 0;
        }

        //This Method calculates the free rentals
        static int CalculateFreeRentals(int totalRentals)
        {
            if (totalRentals >= 10 && totalRentals <= 24)
                return 1;
            else if (totalRentals >= 25 && totalRentals <= 49)
                return 2;
            else if (totalRentals >= 50 && totalRentals <= 74)
                return 4;
            else if (totalRentals >= 75)
                return 8;
            else
                return 0;
        }

        //This method claculates the lost media rewards
        static List<string> CalculateLostMediaRewards(int registrationYear, int currentYear, int totalRentals)
        {
            int customerYears = currentYear - registrationYear;
            List<string> rewards = new List<string>();

            if (customerYears >= 5 && customerYears <= 9 && totalRentals >= 25)
                rewards.Add("1 Bronze-tier");
            else if (customerYears >= 10 && customerYears <= 14 && totalRentals >= 50)
            {
                rewards.Add("3 Bronze-tier");
                rewards.Add("1 Silver-tier");
            }
            else if (customerYears >= 15 && totalRentals >= 75)
            {
                rewards.Add("5 Bronze-tier");
                rewards.Add("2 Silver-tier");
                rewards.Add("1 Gold-tier");
            }

            return rewards;
        }

        //This method displays & processes the menue options
        static void OrderItems(List<OrderItem> orderItems)
        {
            while (true)
            {
                Console.WriteLine("Order Menu:");
                Console.WriteLine("1. Add VHS");
                Console.WriteLine("2. Add DVD");
                Console.WriteLine("3. Add CD");
                Console.WriteLine("4. Done");
                Console.Write("Select an option: ");
                int choice = int.Parse(Console.ReadLine());

                if (choice == 4)
                    break;

                MediaType mediaType = (MediaType)(choice - 1);
                Console.Write("Enter the title: ");
                string title = Console.ReadLine();

                Console.Write("Enter the price: ");
                double price = double.Parse(Console.ReadLine());

                orderItems.Add(new OrderItem { MediaType = mediaType, Title = title, Price = price });
            }
        }

        //This method processes the checkout point
        static void Checkout(List<OrderItem> orderItems, double discount, int freeRentals)
        {
            double total = 0;
            int rentalCount = 0;

            Console.WriteLine("Checkout:");
            foreach (var item in orderItems)
            {
                Console.WriteLine($"{item.MediaType} - {item.Title} - ${item.Price}");
                total += item.Price;
                rentalCount++;
            }

            total -= total * (discount / 100);

            if (rentalCount > freeRentals)
            {
                total -= freeRentals * orderItems[0].Price;
            }

            Console.WriteLine($"Total after discounts and free rentals: ${total:F2}");
        }
    }
}

