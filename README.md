# hotel
using System;
using System.Collections.Generic;

public enum BookingStatus
{
    Processing,
    Cancel,
    Confirm
}

public class Booking
{
    public string Product { get; set; }
    public BookingStatus Status { get; set; }

    public Booking(string product, BookingStatus status)
    {
        Product = product;
        Status = status;
    }
}

public class Itinerary
{
    public List<Booking> Bookings { get; set; }

    public Itinerary()
    {
        Bookings = new List<Booking>();
    }

    public BookingStatus CalculateStatus()
    {
        bool hasProcessing = false;
        bool hasCancel = false;
        bool hasConfirm = false;

        foreach (var booking in Bookings)
        {
            switch (booking.Status)
            {
                case BookingStatus.Processing:
                    hasProcessing = true;
                    break;
                case BookingStatus.Cancel:
                    hasCancel = true;
                    break;
                case BookingStatus.Confirm:
                    hasConfirm = true;
                    break;
            }
        }

        if (hasCancel)
        {
            return BookingStatus.Cancel;
        }
        if (hasProcessing)
        {
            return BookingStatus.Processing;
        }
        if (hasConfirm)
        {
            return BookingStatus.Confirm;
        }

        return BookingStatus.Confirm;
    }
}

class Program
{
    static void Main(string[] args)
    {
        var itinerary = new Itinerary();
        itinerary.Bookings.Add(new Booking("Hotel", BookingStatus.Processing));
        itinerary.Bookings.Add(new Booking("Flight", BookingStatus.Cancel));
        itinerary.Bookings.Add(new Booking("Vehicle", BookingStatus.Confirm));

        BookingStatus itineraryStatus = itinerary.CalculateStatus();
        Console.WriteLine($"Itinerary Status: {itineraryStatus}");
    }
}

