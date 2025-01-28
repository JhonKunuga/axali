using System;
using System.Collections.Generic;
using System.Linq;

class Courier
{
    public string Name { get; set; }
    public string Zone { get; set; }
    public int MaxOrders { get; set; }
    public int CurrentOrders { get; private set; }

    public Courier(string name, string zone, int maxOrders, int currentOrders)
    {
        Name = name;
        Zone = zone;
        MaxOrders = maxOrders;
        CurrentOrders = currentOrders;
    }

    public bool AddOrder()
    {
        if (CurrentOrders < MaxOrders)
        {
            CurrentOrders++;
            return true;
        }
        return false;
    }

    // Operator overloading for ==
    public static bool operator ==(Courier c1, Courier c2)
    {
        return c1.Zone == c2.Zone;
    }

    public static bool operator !=(Courier c1, Courier c2)
    {
        return !(c1 == c2);
    }

    // Operator overloading for < and >
    public static bool operator <(Courier c1, Courier c2)
    {
        return c1.CurrentOrders < c2.CurrentOrders;
    }

    public static bool operator >(Courier c1, Courier c2)
    {
        return c1.CurrentOrders > c2.CurrentOrders;
    }

    public override bool Equals(object obj)
    {
        if (obj is Courier other)
        {
            return this == other;
        }
        return false;
    }

    public override int GetHashCode()
    {
        return Zone.GetHashCode();
    }
}

class Program
{
    static void DistributeOrders(List<Courier> couriers, List<string> orders)
    {
        foreach (var order in orders)
        {
            bool assigned = false;
            foreach (var courier in couriers.OrderBy(c => c.CurrentOrders))
            {
                if (courier.AddOrder())
                {
                    Console.WriteLine($"Order \"{order}\" assigned to {courier.Name} in zone {courier.Zone}.");
                    assigned = true;
                    break;
                }
            }

            if (!assigned)
            {
                Console.WriteLine($"Order \"{order}\" could not be assigned. All couriers are at maximum capacity.");
            }
        }
    }

    static void Main(string[] args)
    {
        // Initialize couriers
        var couriers = new List<Courier>
        {
            new Courier("Courier1", "Vake", 5, 3),
            new Courier("Courier2", "Saburtalo", 4, 2),
            new Courier("Courier3", "Didube", 3, 1)
        };

        // Generate orders
        var orders = new List<string> { "Order1", "Order2", "Order3", "Order4", "Order5" };

        // Distribute orders
        DistributeOrders(couriers, orders);

        // Output courier status
        Console.WriteLine("\nCourier statuses:");
        foreach (var courier in couriers)
        {
            Console.WriteLine($"{courier.Name} ({courier.Zone}): {courier.CurrentOrders}/{courier.MaxOrders} orders.");
        }
    }
}
