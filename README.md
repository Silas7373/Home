using HTLAnmeldung.Data;
using HTLAnmeldung.Models;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.Rendering;
using Microsoft.EntityFrameworkCore;
using System.Diagnostics;

namespace HTLAnmeldung.Controllers
{
    public class HomeController : Controller
    {
        private readonly ILogger<HomeController> _logger;
        private readonly ApplicationDbContext _context;

        public HomeController(ILogger<HomeController> logger, ApplicationDbContext context)
        {
            _logger = logger;
            _context = context;
        }

        public IActionResult Index()
        {
            var model = new RegistrationViewModel
            {
                Departments = _context.departments.ToList()
            };
            return View(model);
        }
        [HttpPost]
        public IActionResult Register(RegistrationViewModel model)
        {
            if (!ModelState.IsValid)
            {
                model.Registration.RegistrationDate = DateTime.Now;
                _context.registration.Add(model.Registration);
                _context.SaveChanges();
                return RedirectToAction("Index");
            }
            model.Departments = _context.departments.ToList();
            return View("Index", model);
        }
        public IActionResult RegisterForm()
        {
            List<SelectListItem> selectListItems = new List<SelectListItem>();
            SelectListItem eintrag0 = new SelectListItem("Informatik", "INF");
            selectListItems.Add(eintrag0);
            SelectListItem eintrag = new SelectListItem("Elektrotechnik", "ET");
            selectListItems.Add(eintrag);
            SelectListItem eintrag2 = new SelectListItem("Bautechnik", "BT");
            selectListItems.Add(eintrag2);
            ViewData["Abteilungswunsch"] = selectListItems;
            return View();
        }

        public IActionResult Privacy()
        {
            return View();
        }

        [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
        public IActionResult Error()
        {
            return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
        }
    }
}
