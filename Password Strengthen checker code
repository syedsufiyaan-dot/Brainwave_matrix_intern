import re

class PasswordStrengthChecker:
    def __init__(self, password):
        self.password = password

    def check_length(self):
        length = len(self.password)
        if length >= 12:
            return 3
        elif length >= 8:
            return 2
        elif length >= 5:
            return 1
        else:
            return 0

    def check_complexity(self):
        score = 0
        if re.search(r'[a-z]', self.password):
            score += 1
        if re.search(r'[A-Z]', self.password):
            score += 1
        if re.search(r'[0-9]', self.password):
            score += 1
        if re.search(r'[^a-zA-Z0-9]', self.password):  
            score += 1
        return score

    def check_uniqueness(self):
        common = ["password", "123456", "qwerty", "admin"]
        if self.password.lower() in common:
            return 0
        if re.match(r'^[a-zA-Z0-9]{1,3}$', self.password):
            return 0
        if len(set(self.password)) <= len(self.password) // 2:
            return 1
        return 2

    def evaluate(self):
        length_score = self.check_length()
        complexity_score = self.check_complexity()
        uniqueness_score = self.check_uniqueness()

        total_score = length_score + complexity_score + uniqueness_score

        if total_score <= 3:
            strength = "Weak"
        elif total_score <= 5:
            strength = "Medium"
        elif total_score <= 7:
            strength = "Strong"
        else:
            strength = "Very Strong"

        return {
            "Password": self.password,
            "Length Score": length_score,
            "Complexity Score": complexity_score,
            "Uniqueness Score": uniqueness_score,
            "Total Score": total_score,
            "Strength": strength,
            "Suggestions": self.suggestions()
        }

    def suggestions(self):
        suggestions = []
        if len(self.password) < 8:
            suggestions.append("Use at least 8 characters.")
        if not re.search(r'[A-Z]', self.password):
            suggestions.append("Add uppercase letters.")
        if not re.search(r'[0-9]', self.password):
            suggestions.append("Include digits.")
        if not re.search(r'[^a-zA-Z0-9]', self.password):
            suggestions.append("Use special symbols (@, #, $, etc.).")
        if self.password.lower() in ["password", "123456", "qwerty", "admin"]:
            suggestions.append("Avoid common dictionary words or patterns.")
        return suggestions if suggestions else ["Good job!"]


# ---------------------- MAIN ----------------------
if __name__ == "__main__":
    pwd = input("Enter a password to check strength: ")
    checker = PasswordStrengthChecker(pwd)
    result = checker.evaluate()

    print("\n--- Password Strength Report ---")
    print(f"Password: {result['Password']}")
    print(f"Strength: {result['Strength']}")
    print(f"Scores -> Length: {result['Length Score']}, Complexity: {result['Complexity Score']}, Uniqueness: {result['Uniqueness Score']}")
    print(f"Total Score: {result['Total Score']} / 9")
    print("Suggestions:")
    for s in result['Suggestions']:
        print(" -", s)
