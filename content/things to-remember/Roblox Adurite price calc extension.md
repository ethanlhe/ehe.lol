```
def parse_price(price):
    return int(price.replace('$', ''))

def parse_rap(rap):
    return int(float(rap[:-1]) * 1000)  # Convert K to thousands

def knapsack(items, budget):
    # Convert prices and RAPs to integers
    items = [{'name': item['name'], 'rap': parse_rap(item['rap']), 'price': parse_price(item['price'])} for item in items]
    
    # Create a DP table where dp[i] is the max RAP we can get with budget i
    dp = [0] * (budget + 1)
    
    # Fill the DP table
    for item in items:
        for j in range(budget, item['price'] - 1, -1):
            dp[j] = max(dp[j], dp[j - item['price']] + item['rap'])

    # Find the combination of items that gives the max RAP
    result_rap = dp[budget]
    current_budget = budget
    selected_items = []

    for item in reversed(items):
        if current_budget >= item['price'] and result_rap == dp[current_budget] - dp[current_budget - item['price']] + item['rap']:
            selected_items.append(item['name'])
            current_budget -= item['price']
            result_rap -= item['rap']
            if result_rap == 0:
                break

    return selected_items, dp[budget]

# List of items
items = [
    {'name': 'Dr. Spooks Magic Top Hat', 'rap': '1.2K', 'price': '$3'},
    {'name': 'Swordpack', 'rap': '1.2K', 'price': '$5'},
    {'name': 'Swordpack', 'rap': '1.2K', 'price': '$3'},
    {'name': 'Noob Attack: Golden Sword Gladiator', 'rap': '1.2K', 'price': '$6'},
    {'name': 'Noob Attack: Golden Sword Gladiator', 'rap': '1.2K', 'price': '$3'},
    # Add other items similarly...
]

selected_items, total_rap = knapsack(items, 100)
print("Selected items:", selected_items)
print("Total RAP:", total_rap)

```
Parse the best items to get with a certain budget to maximize profit, knapsack problem.

Could make into chrome extension