# Movie Ticket Booking System

## Requirement Gathering
1. User should login and book a ticket.
2. Choose Movie, Seat and Pay
3. Refund the Ticket

Use logins into the system, see the list of movies with show time. Choose a show time with Movie, select seat, pay the price and get tickets.

User, Movie, Seat, Ticket, Show, Theater

User
    id
    loggedIn

Movie
    name
    language (Enum)
    genre (Enum)

SeatType (Enum)
    Regular
    VIP
    DBox

Seat
    SeatType
    SeatNo

Ticket
    seat
    Show
    amount
    userId
    theatre

Show
    Movie
    showtime
    availableSeats

    updateAvailableSeats()

Theater
    Name
    Id
    Address
    List<Show> shows;

Booking
    paymentStrategy;
    notificationListener;
    book()
        BookingHandler (Abstract class)
            TheatreHandler -> generateEvent(Ticket )
            ShowHandler -> generateEvent(Ticket )
            SeatHandler -> generateEvent(Ticket )
        paymentStrategy.processPayment();
        notificationListener.notify();
    
PaymentStrategy (I) (Strategy Pattern)
    CardPayment -> Credit Card, Gift Card, Debit Card
        processPayment(Ticket)
            getCardInfo()
    CashPayment
        processPayment(Ticket)

NotificationListener  (ObserverPattern)
    EmailListener
    SMSListener
    PrintListener


If purchasing snacks allowed then - we can implement DecoratorPattern







