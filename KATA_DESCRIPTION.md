# Code Kata: Phil's Quick Lube Management System

## The Business Story

### Welcome to Phil's Quick Lube

Phil's Quick Lube is a preventive vehicle maintenance service business located in Texas. Unlike traditional auto repair shops that fix broken parts, Phil's specializes exclusively in preventive maintenance—the kind of routine services that keep vehicles running smoothly and prevent problems before they occur. Think oil changes, battery checks, fluid top-offs, and state inspections. No repairs, no diagnostics on check engine lights, just fast, reliable preventive care.

The business has built a reputation on speed and convenience. Customers can drive in without an appointment (walk-in only), and the goal is to get them back on the road as quickly as possible. For most services, especially oil changes, turnaround time is critical to customer satisfaction.

### The Services

Phil's Quick Lube offers five core services:

1. **Oil Changes** — The bread and butter of the business. This is the highest volume service, with approximately 10 oil changes completed on a typical day. Customers come in regularly (every 3,000-5,000 miles or 3-6 months), and many are repeat customers.

2. **Battery Services** — Checking battery health, cleaning terminals, and replacing batteries when needed. About 6 battery services per day on average.

3. **Transmission Services** — Draining old transmission fluid, replacing the filter, and adding fresh fluid. This is a longer service (about 2 hours) and is performed less frequently.

4. **Cooling System Services** — Inspecting the cooling system, checking hoses and radiator, and replenishing coolant. Takes about 45 minutes.

5. **Texas State Vehicle Inspections** — Annual safety and emissions inspections required by the state. About 9 inspections per day. These must follow specific state-mandated procedures and documentation.

### The Team Structure

Phil's Quick Lube operates with 14 staff members across three departments:

**Sales Department (3 staff members)**
- The customer-facing team
- Greets walk-in customers at the front desk
- Creates service orders by collecting customer and vehicle information
- Explains services and pricing
- Prepares invoices when service is complete
- Collects payments (cash, card, check, digital wallet)
- Handles customer questions and concerns

**Service Department (5 technicians + 2 managers = 7 staff)**
- The hands-on team that performs the actual maintenance work
- Works in three specialized service bays:
  - **Bay 1:** Dedicated to oil changes only (the highest volume service)
  - **Bay 2:** Handles battery work and fluid replenishment services
  - **Bay 3:** Reserved for state vehicle inspections (with special equipment)
- Technicians are assigned to bays based on availability
- Service managers oversee quality and can step in to perform work when needed
- After completing work, service staff performs quality checks and installs reminder stickers on windshields

**Administration Department (3 staff members)**
- The behind-the-scenes team managing suppliers and finances
- Maintains relationships with auto parts suppliers (oil, filters, batteries, fluids)
- Creates and tracks purchase orders for inventory
- Processes supplier invoices and manages accounts payable
- Coordinates deliveries and inventory replenishment

**Owner/Manager (1 person)**
- Oversees all operations and has access to all systems
- Reviews performance metrics and makes strategic decisions

### The Current Customer Experience

Let's walk through what currently happens when a customer arrives for an oil change:

**9:15 AM - Customer Arrives**

Sarah pulls into Phil's Quick Lube for an oil change. She's been here before—this is actually her third visit in the past 18 months. A sales staff member greets her at the front desk with a friendly smile.

**9:17 AM - The Information Gathering Dance**

The sales person asks Sarah for her information: name, phone number, address, email. Sarah recites it all, even though she's given this exact information twice before. The employee types it into the computer system's "Vehicle Information entry screen."

Next comes the vehicle details: license plate, make, model, year, current mileage. The employee walks outside to verify the license plate, walks back in, types it in. Then walks outside again to check the mileage through the windshield, walks back in, types that in too.

Sarah watches this back-and-forth with mild frustration. "Don't you have my information from last time?" she asks. "We do," the employee explains apologetically, "but our system doesn't make it easy to pull up, so it's usually faster to just re-enter everything." Sarah sighs.

**9:25 AM - Service Order Created**

After 10 minutes of data entry and verification, the system prints out a service order. The employee places it in a physical manila folder along with Sarah's car keys—this is called a "job packet." The employee walks the packet over to a staging area and places it in a queue for Service.

Sarah is told her car will be ready in about 30-45 minutes. She takes a seat in the small waiting area.

**9:30 AM - The Service Department Discovers the Work**

A service technician in Bay 1 finishes the previous oil change, walks over to the staging area, and discovers Sarah's job packet waiting. He picks it up, reads the service order, and walks outside to find Sarah's vehicle in the parking lot.

**9:35 AM - Service Begins**

The technician drives Sarah's car into Bay 1, raises it on the lift, and begins the oil change. While working, he realizes they're running low on a specific oil filter size. He writes out a quick material request note and sets it aside to hand to Administration later—but in the rush of work, he forgets about it temporarily.

**10:00 AM - Service Complete**

The oil change is done. The technician lowers the vehicle, performs a quick quality check (making sure the drain plug is tight, the oil level is correct, no leaks), and places a reminder sticker on the windshield showing when the next oil change is due.

He drives the car to the pickup area, writes "READY" on the job packet, and walks it back to the front desk.

**10:05 AM - Invoice and Payment**

The sales person sees the completed job packet, creates an invoice in yet another system, prints it, and calls Sarah's name. Sarah comes to the counter, reviews the invoice showing $79.99 plus 3% Texas sales tax for a total of $82.39, pays with her credit card, and receives a receipt.

**10:10 AM - Sarah Drives Away**

Total time: 55 minutes from arrival to departure. Not terrible, but not great either. Sarah leaves feeling the service was fine, but wondering why she had to give all her information again when she's clearly a repeat customer.

### The Problems Hiding in Plain Sight

Phil, the owner, has spent years running this business, and he knows something isn't right. Reviewing his operations with a process improvement consultant revealed several critical inefficiencies:

#### Problem 1: Death by a Thousand Redundancies

The "Write Service Order" activity—that initial 10 minutes with Sarah—is a masterclass in waste:

- **Excessive Motion:** The sales staff member physically walks outside multiple times to verify license plate and check mileage, then walks back inside to type it in. This happens for every single customer, multiple times per day. That's a lot of walking.

- **Redundant Data Entry:** For returning customers (like Sarah, who represents about 70% of daily traffic), the system requires re-entering all information from scratch. The data exists in the system, but retrieving it is so cumbersome that employees find it faster to just re-enter everything.

- **Multiple Decision Points:** The employee makes the same inspection decisions repeatedly—check the license plate, check the mileage, confirm the information—each time adding small delays that compound across dozens of customers per day.

#### Problem 2: The Paper Trail of Tears

The physical job packet system creates its own cascade of problems:

- **Manual Handoffs:** The manila folder with service order and keys must physically move from Sales to Service to back to Sales. Each handoff requires someone to walk somewhere and hand something to someone else—or leave it in a staging area hoping the right person finds it at the right time.

- **Zero Visibility:** When Sarah asks "How much longer?" at the 35-minute mark, the sales staff has no idea. They can't see into the service bay computers (if the bays even have them). They have to walk back to the service area, find someone, ask about the status, walk back, and report. Real-time status? Forget about it.

- **Synchronization Chaos:** The handoffs are rarely synchronized. Job packets sit in queues waiting for someone to notice them. Sometimes they're misplaced. Sometimes they're taken out of order. The system depends entirely on manual attention and physical presence.

- **Risk of Loss:** A physical folder can be lost, damaged, or misplaced. When it disappears, chaos ensues—keys are missing, service details are unclear, customer information is gone.

#### Problem 3: Inventory Guesswork

Phil's Quick Lube orders supplies weekly in batches. An administration staff member literally walks around the shop with a clipboard, eyeballing inventory levels, and guessing what to order:

- **No Real-Time Tracking:** There's no system that knows exactly how much oil, how many filters, how many batteries are in stock right now. It's all visual estimation and manual counting.

- **Reactive Ordering:** Remember that technician who ran low on oil filters mid-service? The manual material request note system is supposed to alert Administration, but it's entirely dependent on technicians remembering to write notes and physically delivering them. Sometimes requests are forgotten. Sometimes they're delayed.

- **Weekly Batches:** Instead of ordering based on actual consumption patterns and forecasting, Phil's orders supplies on a weekly schedule. This leads to overstocking (wasted capital tied up in inventory) or understocking (delays in service when parts aren't available).

- **No Automated Reordering:** When supplies hit a critical threshold, there's no automatic alert or purchase order creation. Someone has to notice, remember, and manually create a PO.

#### Problem 4: The Returning Customer Paradox

This is perhaps the most frustrating problem from the customer's perspective:

- **Repeat the Same Song and Dance:** Like Sarah, repeat customers must provide all their information every single time as if they're brand new. It's frustrating, time-consuming, and makes customers feel like strangers even after years of loyalty.

- **No Historical Context:** When a customer says "Just do what you did last time," the sales staff has to hunt through paper records or dig through a poorly indexed database to figure out what "last time" actually was.

- **Missed Opportunities:** The system doesn't prompt for service reminders based on history. A customer who gets an oil change every 3 months won't receive a proactive reminder—they have to remember themselves and come back.

### The Business Impact

These inefficiencies aren't just annoying—they're costly:

- **Time Waste:** The excessive motion and redundant data entry in service order creation alone adds 5-10 minutes per customer. Multiplied across 25 customers per day, that's 125-250 minutes (2-4 hours) of wasted staff time daily.

- **Customer Friction:** Returning customers feel unrecognized and undervalued when they must repeat information. This impacts satisfaction and retention.

- **Inventory Costs:** Without real-time tracking and automated reordering, Phil either overstocks (tying up cash in inventory) or understocks (causing service delays and lost revenue).

- **Missed Revenue:** When services are delayed due to inventory shortages or scheduling confusion, revenue is lost. When customers have poor experiences, they don't return.

- **Operational Inefficiency:** Manual handoffs and paper-based tracking slow down the entire operation. The lack of visibility across departments means problems aren't discovered until they become critical.

- **Quality Issues:** Without systematic quality checks and digital checklists, service quality varies by technician. Mistakes can slip through, leading to rework or customer complaints.

### The Vision

Phil wants to transform his business with a comprehensive management system that:

1. **Eliminates Redundancy:** Returning customers should be recognized instantly. A phone number lookup should pull up their entire history—past vehicles, past services, contact information—ready to confirm rather than re-enter.

2. **Enables Real-Time Visibility:** Everyone should know the status of every service order at any moment. Sales staff should be able to tell customers "Your car is in Bay 1, about 15 minutes remaining" without leaving the front desk.

3. **Automates Inventory Management:** The system should track every oil change, every filter used, every battery installed, and automatically trigger reorders when supplies hit minimum thresholds.

4. **Digitizes the Workflow:** No more paper job packets. Service orders should flow digitally from Sales to Service to Administration, with each department seeing exactly what they need when they need it.

5. **Improves Customer Experience:** Faster check-in for returning customers, digital service reminders, clear communication of status, and smooth payment processing.

6. **Provides Business Intelligence:** Phil wants to see metrics: average turnaround time, service volumes, bay utilization, technician performance, customer retention, inventory turn rates. Data-driven decisions instead of gut feelings.

### Key Business Rules & Constraints

As you design and build this system, keep these critical business rules in mind:

**Service Bay Routing:**
- Oil changes ALWAYS go to Bay 1 (never Bay 2 or Bay 3)
- Battery and fluid services ALWAYS go to Bay 2 (never Bay 1 or Bay 3)
- State inspections ALWAYS go to Bay 3 (never Bay 1 or Bay 2)
- If a customer orders multiple services (e.g., oil change + battery), route to the lowest bay number first (Bay 1 for oil, then Bay 2 for battery)

**Service Order States:**
- Orders flow through states: Created → Approved (optional skip) → In Progress → Quality Check (Phase 2+) → Ready for Pickup → Completed
- Inventory is deducted when an order moves to Approved state (or implicitly when skipping straight to In Progress)
- Quality checks (once implemented in Phase 2+) cannot be skipped—all services must pass inspection
- Failed quality checks send the order back to In Progress with a rework indicator
- If a service fails quality check twice, the service manager must personally handle the rework

**Inventory Management (Critical):**
- Supplies are deducted when a service order is APPROVED, not when it's completed
- Low stock alerts must prevent service order approval if supplies are insufficient (cannot go negative)
- Service managers CAN override low-stock blocks with explicit confirmation and audit logging
- For multi-service orders, if only some services have sufficient inventory, the system auto-splits the order at approval time
- Services without inventory templates defined can still proceed (deduction is skipped)

**Returning Customer Recognition:**
- A "returning customer" is any customer with an existing record in the database
- Phone number lookup is the primary method for auto-population
- If multiple customers share a phone number, display all matches and require explicit selection
- Display all vehicles for a returning customer in a dropdown; require explicit selection (never auto-select, even if only one vehicle)
- User must confirm all auto-populated data before proceeding

**Pricing & Payments:**
- Fixed pricing: Oil Change $79.99, Battery $149.99, Transmission $249.99, Cooling $199.99, Inspection $25.50
- Texas sales tax: 3% applied to subtotal
- Service managers can apply flat dollar discounts only (no percentage discounts)
- Payment is BLOCKED until all services in a multi-service order are complete
- No partial payments, no voids, no refunds after payment is processed
- Payment methods: cash, credit/debit card, check, digital wallet
- Zero-cost complimentary services do not require payment processing

**Business Hours:**
- Monday-Saturday, 6:00 AM to 6:00 PM
- Service orders can only be CREATED and APPROVED during business hours
- System remains available 24/7 for other operations (viewing status, reports, etc.)

**Data & Security:**
- User roles: Administrator, Sales Staff, Service Technician, Service Manager, Administration Staff, Owner/Manager
- Service technicians can only see their own assigned orders (not other technicians' work)
- Service managers can view all orders and reassign work at any time
- All data modifications and login/logout events must be audit logged
- Audit logs retained for 7 years (same as transaction data)
- Session timeout after 15 minutes of inactivity
- Password requirements: minimum 8 characters, 1 uppercase, 1 lowercase, 1 number

**No Repairs:**
- This system is for preventive maintenance ONLY
- Do not include features for vehicle repairs, diagnostic work, or fixing broken components

### Success Metrics

Phil has clear goals for what success looks like:

- **60% reduction** in service order creation time for returning customers (from ~10 minutes to ~4 minutes)
- **15% reduction** in average service turnaround time (better flow, less waiting)
- **Zero manual handoffs** of paper job packets (fully digital workflow)
- **95% on-time service completion** (services completed within estimated time)
- **Zero inventory stockouts** (no services delayed due to missing supplies)
- **85%+ bay utilization** during business hours (bays actively used, not sitting idle)

### The Challenge

Your mission, should you choose to accept it, is to design and build the Phil's Quick Lube Management System (PQLMS)—a comprehensive software solution that eliminates these inefficiencies, streamlines operations, and transforms the customer experience.

This is a code kata focused on business domain modeling, workflow automation, inventory management, and real-world operational constraints. You'll need to model customers, vehicles, service orders, inventory, suppliers, invoicing, payments, users, and more—all while respecting the business rules and achieving the success metrics Phil has defined.

The system will be built in phases:

- **Phase 1 (Core Operations):** Customer/vehicle management, service order creation and tracking, basic invoicing/payment, user management, inventory tracking
- **Phase 2 (Service Execution):** Digital checklists for oil changes, bay management, quality checks, real-time status
- **Phase 3 (Back Office):** Advanced inventory management, supplier management, purchase orders, accounting integration
- **Phase 4 (Analytics):** Reporting dashboard, performance analytics, forecasting
- **Phase 5 (Advanced Features):** Appointment scheduling, customer portal, automated reordering

Start with Phase 1, prove the core functionality works, and expand from there.

Good luck, and may your code be as smooth as fresh synthetic oil.

---

*This kata is based on the real-world case study of Phil's Quick Lube from "The Basics of Process Mapping, 2nd Edition" by Robert Damelio, adapted for software development practice.*
