# Calorie-tracking-app-
Application lets user track calories and macros 
pip install Flask
from flask import Flask, render_template, request

app = Flask(__name__)

class MacroTracker:
    def __init__(self):
        self.meals = []

    def add_meal(self, meal):
        self.meals.append(meal)

    def calculate_totals(self):
        calories = sum(meal['calories'] for meal in self.meals)
        protein = sum(meal['protein'] for meal in self.meals)
        carbs = sum(meal['carbs'] for meal in self.meals)
        fat = sum(meal['fat'] for meal in self.meals)
        return calories, protein, carbs, fat

macro_tracker = MacroTracker()

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        meal = {
            'name': request.form['name'],
            'calories': int(request.form['calories']),
            'protein': float(request.form['protein']),
            'carbs': float(request.form['carbs']),
            'fat': float(request.form['fat']),
        }

        macro_tracker.add_meal(meal)

    totals = macro_tracker.calculate_totals()

    return render_template('index.html', meals=macro_tracker.meals, totals=totals)

if __name__ == '__main__':
    app.run(debug=True)
