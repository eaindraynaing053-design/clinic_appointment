# MediCare Clinic Appointment System

## Premium CarePoint interface

The shared `assets/css/premium.css` layer applies a consistent deep-navy and teal medical design across the public home page, authentication, admin, doctor, and patient pages. It adds responsive layouts, accessible focus styles, elevated cards and buttons, subtle 3D hover depth, and the existing professional clinic photography. The photography is loaded from Unsplash and requires an internet connection; the app itself remains local.

The shared `assets/js/script.js` file adds accessible password visibility toggles, appointment-slot selection feedback, character counters for limited text fields, and double-submit protection for POST forms. It is vanilla JavaScript and does not need a library.

## Run locally with XAMPP

1. Start **Apache** and **MySQL** in the `C:\xampp8\xampp-control.exe` XAMPP Control Panel.
2. The repaired project is installed at `C:\xampp8\htdocs\clinic_appointment`.
3. In phpMyAdmin, confirm the database is named `clinic_db` and contains the existing tables: `users`, `departments`, `doctors`, `doctor_schedules`, `days_off`, `appointments`, `visit_notes`, and `notifications`.
4. Visit `http://localhost/clinic_appointment/admin/setup_admin.php` and create the first administrator. The setup page locks itself after an admin exists.
5. Register a patient at `http://localhost/clinic_appointment/auth/register.php`, sign in, add departments/doctors/schedules as admin, then book from the patient dashboard.

The app connects as XAMPP's default `root` user with a blank password in `config/database.php`. Change those settings if the local MySQL account differs. Existing database records and tables are preserved.

## Optional patient age field

The profile page detects whether `users.age` is present. The active `clinic_db` already has this column. For an older copy that does not, confirm it is absent before running `database/001_add_patient_age.sql` once; it only adds a nullable column.

The original database's strict doctor/date/time index has been replaced in the active `clinic_db` with an active-status-only unique index. This lets canceled and rejected slots reopen while preserving appointment rows. For another untouched database copy that still has the `unique_doctor_slot` index, `database/002_active_slot_unique.sql` applies that one-time index repair.

## Core behavior

- Patient registration creates only `patient` accounts; administrators are created separately.
- Booking slots are generated from weekly schedules, excluding days off, past times, and pending/approved appointments.
- The database's active-slot unique key provides the final double-booking guard.
- Admins can filter requests, approve/reject with a reason, reschedule to an open slot, and mark approved visits complete.
- Patient status changes and cancellations are recorded in the existing notifications table.
- Patients can cancel pending or approved visits up to 24 hours before the appointment and review completed visit notes.

## Quick demonstration

1. Admin: create a department, doctor, and a schedule for a weekday that has not passed yet.
2. Patient: register, browse the doctor, select the matching date, and book one open slot.
3. Admin: filter for pending requests, approve the request, then verify the patient's notification.
4. Admin: reschedule to another open slot and confirm the updated date appears for the patient.
5. Mark the appointment complete, add a visit note, and open the patient's medical history.
