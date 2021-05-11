# LiveProject-CSharp
The Tech Academy C# Live Project

# Introduction

During this project, I worked with a team of other developers to create a dyanmic website using ASP .Net MVC and Entity Framework. While working on the code itself, I also gained experience utilizing the agile methodology for software development. We participated in daily stand-up meetings where we discussed the trials we were facing and what we were working on until the next meeting. We also had a code retrospective at the end of our 10 day sprint where we discussed our successes and areas in which we needed to improve. This project was intended to fill the gap between schooling and actually being a part of a development team, and I feel that the experience gained during this two weeks will be invaluable in my career.

The website is still in its early stages and isn't fully available. I'll post code snippets from my work below instead.

# Creating the Calendar Events Class

The first story on the project was to create an entity model for the Calendar Event class. I was also assigned to create CRUD pages for this model.

# Calendar Event Model

```

     public class CalendarEvent
    {
        [Key]
        public int EventId { get; set; }
        public string Title { get; set; }
        [DataType(DataType.Date)]
        [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
        public DateTime StartDate { get; set; }
        [DataType(DataType.Date)]
        [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
        public DateTime EndDate { get; set; }
        [DataType(DataType.Time)]
        [DisplayFormat(DataFormatString = "{0:HH:mm}", ApplyFormatInEditMode = true)]
        public DateTime? StartTime { get; set; }
        [DataType(DataType.Time)]
        [DisplayFormat(DataFormatString = "{0:HH:mm}", ApplyFormatInEditMode = true)]
        public DateTime? EndTime { get; set; }
        public bool AllDay { get; set; }
        public int TicketsAvailable { get; set; }
        public bool IsProduction { get; set; }
    }

```
# Create and Seed Event Planner User

The following two stories involved creating a specific user, EventPlanner, for this calendar event model and seeding this user.
```

    public class EventPlanner : ApplicationUser
    {
        public DateTime PlannerStartDate { get; set; }
        public static void Seed(ApplicationDbContext db)
        {
            var roleManager = new RoleManager<IdentityRole>(new RoleStore<IdentityRole>(db));
            var userManager = new UserManager<ApplicationUser>(new UserStore<ApplicationUser>(db));
            if (!roleManager.RoleExists("EventPlanner"))
            {
                var role = new IdentityRole()
                {
                    Name = "EventPlanner"
                };
                roleManager.Create(role);
                var eventPlanner = new EventPlanner()
                {
                    UserName = "EventPlanner",
                    Email = "plannerofevents@email.com",
                    PlannerStartDate = new DateTime(2021,1,1),
                };
                string password = "*********";
                var checkPlanner = userManager.Create(eventPlanner, password);
                if (checkPlanner.Succeeded)
                {
                    userManager.AddToRole(eventPlanner.Id, "EventPlanner");
                }
            }
        }
    }

```
# Style the Create and Edit Pages

The final story I worked on during this sprint was to style the already scaffolded create and edit pages to certain required specifications. In addition to the following code snippet I utilized some bootstrap classes for some minor styling of the forms.

```

.calendarEvent-Container {
    max-width: 650px;
    margin: auto;
}
.calendarEvent-form {
    max-width: 650px;
    color: var(--dark-color);
    text-align: left;
    background-color: var(--light-color);
    border-radius: 20px;
    border-color: var(--secondary-color);
    padding-top: 20px;
    padding-bottom: 10px;
}
.calendarEvent-header {
    padding-left: 10px;
}
.calendarEventForm-control {
    display: block;
    width: 100%;
    height: calc(1.5em + 0.75rem + 2px);
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    font-weight: 400;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}
.calendarEventForm-control:focus {
    color: #495057;
    background-color: #fff;
    border-color: var(--dark-color);
    outline: 0;
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(0, 0, 0, 0.5);
}

```

