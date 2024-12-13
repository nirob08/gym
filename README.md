import tkinter as tk
from tkinter import messagebox

class Member:
    def __init__(self, member_id, name, age, membership_type, join_date):
        self.member_id = member_id
        self.name = name
        self.age = age
        self.membership_type = membership_type
        self.join_date = join_date
        self.fees_paid = False
    
    def pay_fees(self):
        self.fees_paid = True

    def display_member(self):
        status = "Paid" if self.fees_paid else "Not Paid"
        return f"ID: {self.member_id}, Name: {self.name}, Age: {self.age}, Membership Type: {self.membership_type}, Join Date: {self.join_date}, Fees: {status}"

class GymManagementSystem:
    def __init__(self):
        self.members = []

    def add_member(self, member_id, name, age, membership_type, join_date):
        member = Member(member_id, name, age, membership_type, join_date)
        self.members.append(member)
        return f"Member {name} added successfully."

    def display_members(self):
        if not self.members:
            return "No members found!"
        return "\n".join([member.display_member() for member in self.members])

    def pay_member_fees(self, member_id):
        member = next((m for m in self.members if m.member_id == member_id), None)
        if member:
            member.pay_fees()
            return f"Fees for {member.name} have been paid."
        else:
            return "Member not found!"

    def search_member(self, member_id):
        member = next((m for m in self.members if m.member_id == member_id), None)
        if member:
            return member.display_member()
        else:
            return "Member not found!"

    def remove_member(self, member_id):
        member = next((m for m in self.members if m.member_id == member_id), None)
        if member:
            self.members.remove(member)
            return f"Member {member.name} removed successfully."
        else:
            return "Member not found!"

class GymManagementUI:
    def __init__(self, root, system):
        self.system = system
        self.root = root
        self.root.title("IS Gym Management System")
        self.root.configure(bg="#d32f2f")  # Red background for the root window

        # Add Member Section
        self.label_add_member = tk.Label(root, text="Add Member", bg="#d32f2f", fg="white", font=("Helvetica", 14))
        self.label_add_member.grid(row=0, column=0, columnspan=2, pady=10)

        self.label_id = tk.Label(root, text="Member ID:", bg="#d32f2f", fg="white")
        self.label_id.grid(row=1, column=0)
        self.entry_id = tk.Entry(root)
        self.entry_id.grid(row=1, column=1)

        self.label_name = tk.Label(root, text="Name:", bg="#d32f2f", fg="white")
        self.label_name.grid(row=2, column=0)
        self.entry_name = tk.Entry(root)
        self.entry_name.grid(row=2, column=1)

        self.label_age = tk.Label(root, text="Age:", bg="#d32f2f", fg="white")
        self.label_age.grid(row=3, column=0)
        self.entry_age = tk.Entry(root)
        self.entry_age.grid(row=3, column=1)

        self.label_membership = tk.Label(root, text="Membership Type (Basic/Premium):", bg="#d32f2f", fg="white")
        self.label_membership.grid(row=4, column=0)
        self.entry_membership = tk.Entry(root)
        self.entry_membership.grid(row=4, column=1)

        self.label_join_date = tk.Label(root, text="Join Date (DD/MM/YYYY):", bg="#d32f2f", fg="white")
        self.label_join_date.grid(row=5, column=0)
        self.entry_join_date = tk.Entry(root)
        self.entry_join_date.grid(row=5, column=1)

        self.button_add = tk.Button(root, text="Add Member", command=self.add_member, bg="#1976d2", fg="white")
        self.button_add.grid(row=6, column=0, columnspan=2, pady=10)

        # Display Members Section
        self.label_display = tk.Label(root, text="All Members:", bg="#d32f2f", fg="white", font=("Helvetica", 14))
        self.label_display.grid(row=7, column=0, columnspan=2)

        self.text_display = tk.Text(root, height=10, width=50, bg="white", fg="black")
        self.text_display.grid(row=8, column=0, columnspan=2, pady=10)

        # Pay Fees Section
        self.label_pay_fees = tk.Label(root, text="Pay Fees (Enter Member ID):", bg="#d32f2f", fg="white")
        self.label_pay_fees.grid(row=9, column=0)
        self.entry_pay_fees = tk.Entry(root)
        self.entry_pay_fees.grid(row=9, column=1)

        self.button_pay_fees = tk.Button(root, text="Pay Fees", command=self.pay_fees, bg="#1976d2", fg="white")
        self.button_pay_fees.grid(row=10, column=0, columnspan=2, pady=10)

        # Search Member Section
        self.label_search = tk.Label(root, text="Search Member by ID:", bg="#d32f2f", fg="white")
        self.label_search.grid(row=11, column=0)
        self.entry_search = tk.Entry(root)
        self.entry_search.grid(row=11, column=1)

        self.button_search = tk.Button(root, text="Search Member", command=self.search_member, bg="#1976d2", fg="white")
        self.button_search.grid(row=12, column=0, columnspan=2, pady=10)

        # Remove Member Section
        self.label_remove = tk.Label(root, text="Remove Member by ID:", bg="#d32f2f", fg="white")
        self.label_remove.grid(row=13, column=0)
        self.entry_remove = tk.Entry(root)
        self.entry_remove.grid(row=13, column=1)

        self.button_remove = tk.Button(root, text="Remove Member", command=self.remove_member, bg="#1976d2", fg="white")
        self.button_remove.grid(row=14, column=0, columnspan=2, pady=10)

    def add_member(self):
        member_id = self.entry_id.get()
        name = self.entry_name.get()
        age = self.entry_age.get()
        membership_type = self.entry_membership.get()
        join_date = self.entry_join_date.get()
        
        if not member_id or not name or not age or not membership_type or not join_date:
            messagebox.showwarning("Input Error", "Please fill all fields.")
            return

        result = self.system.add_member(member_id, name, int(age), membership_type, join_date)
        messagebox.showinfo("Success", result)
        self.clear_entries()

    def display_members(self):
        self.text_display.delete(1.0, tk.END)
        result = self.system.display_members()
        self.text_display.insert(tk.END, result)

    def pay_fees(self):
        member_id = self.entry_pay_fees.get()
        result = self.system.pay_member_fees(member_id)
        messagebox.showinfo("Payment", result)

    def search_member(self):
        member_id = self.entry_search.get()
        result = self.system.search_member(member_id)
        messagebox.showinfo("Search Result", result)

    def remove_member(self):
        member_id = self.entry_remove.get()
        result = self.system.remove_member(member_id)
        messagebox.showinfo("Remove Member", result)

    def clear_entries(self):
        self.entry_id.delete(0, tk.END)
        self.entry_name.delete(0, tk.END)
        self.entry_age.delete(0, tk.END)
        self.entry_membership.delete(0, tk.END)
        self.entry_join_date.delete(0, tk.END)

def main():
    system = GymManagementSystem()
    root = tk.Tk()
    ui = GymManagementUI(root, system)

    button_display = tk.Button(root, text="Display All Members", command=ui.display_members, bg="#1976d2", fg="white")
    button_display.grid(row=15, column=0, columnspan=2, pady=10)

    root.mainloop()

if __name__ == "__main__":
    main()
