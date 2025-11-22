import time
import sys
import random

# -----------------------------------------------------------------------------
# 1. UI/UX DESIGN (Terminal/Console Version)
# -----------------------------------------------------------------------------

class Colors:
    HEADER = '\033[95m'
    BLUE = '\033[94m'
    CYAN = '\033[96m'
    GREEN = '\033[92m'
    WARNING = '\033[93m'
    FAIL = '\033[91m'
    ENDC = '\033[0m'
    BOLD = '\033[1m'
    UNDERLINE = '\033[4m'

def print_slow(text, delay=0.02):
    """Simulate typing effect for better UX"""
    for char in text:
        sys.stdout.write(char)
        sys.stdout.flush()
        time.sleep(delay)
    print()

def print_header():
    print(Colors.HEADER + Colors.BOLD + "="*60)
    print("✨  LUMINA  |  Intelligent Consumer Care Guide")
    print("="*60 + Colors.ENDC)
    print(Colors.CYAN + "System: Initializing AI interface..." + Colors.ENDC)
    time.sleep(1)
    print(Colors.CYAN + "System: Ready." + Colors.ENDC + "\n")

# -----------------------------------------------------------------------------
# 2. CHATBOT LOGIC (THE BRAIN)
# -----------------------------------------------------------------------------

def get_lumina_response(prompt, context):
    prompt = prompt.lower()
    
    # Get current state, default to None
    state = context.get('state')

    # Simulate processing time
    time.sleep(0.5)

    # Greeting
    if any(word in prompt for word in ["hello", "hi", "hey", "start"]):
        context['state'] = None # Reset state on new greeting
        return f"Hello {context['user']}! I am {Colors.HEADER}Lumina✨{Colors.BLUE}. I'm here to guide you through your support request. How can I help you today?"

    # Refund Logic
    if "refund" in prompt:
        context['state'] = 'refund_flow'
        if "order id" not in prompt and "number" not in prompt:
            return "I can certainly help with a refund. To proceed, could you please provide your " + Colors.BOLD + "Order ID" + Colors.BLUE + " (e.g., ORD-12345)?"
        elif "ord-" in prompt:
            # Extract simple ID simulation
            return f"Thank you. I've located order {Colors.BOLD}{prompt.upper().split('ORD-')[1][:5]}{Colors.BLUE}. I have processed a refund request. You should receive a confirmation email within 24 hours."
        else:
            return "Please allow me to assist with your return. Is the item damaged, or did you simply change your mind?"

    # Technical Support Logic
    if "broken" in prompt or "not working" in prompt:
        context['state'] = 'tech_support_restart'
        return "I'm sorry to hear that. Let's troubleshoot. First, have you tried restarting the device by holding the power button for 10 seconds?"

    # Context-aware "Yes" / "Tried that"
    if state == 'tech_support_restart' and ("tried that" in prompt or "yes" in prompt):
        context['state'] = 'tech_support_connection'
        return "Okay, thank you for trying. Since a restart didn't fix it, let's check the connection. Is the status light blinking Red or Green?"

    # Billing Logic
    if "bill" in prompt or "charge" in prompt:
        context['state'] = 'billing_flow'
        return "I can help clarify your billing statement. Are you seeing an unrecognized charge, or do you need a copy of your invoice?"

    # Gratitude
    if "thank" in prompt:
        return "You are very welcome! It's my pleasure to help. ✨"

    # Fallback
    return "I understand. To ensure you get the best assistance, could you provide a few more details? Or type 'agent' to speak to a human."

# -----------------------------------------------------------------------------
# 3. MAIN LOOP
# -----------------------------------------------------------------------------

def main():
    print_header()

    # Simple Context Gathering
    print(Colors.GREEN + "Lumina: Before we begin, may I have your name?" + Colors.ENDC)

    try:
        user_name = input(Colors.WARNING + "You: " + Colors.ENDC).strip()
    except EOFError:
        print(Colors.WARNING + "You: (Auto-Guest)" + Colors.ENDC)
        user_name = "Guest"

    if not user_name: user_name = "Guest"

    context = {"user": user_name, "state": None}

    print_slow(f"\n{Colors.BLUE}Lumina: Welcome, {user_name}. I can help with Refunds, Tech Support, or Billing.{Colors.ENDC}")

    while True:
        try:
            user_input = input(f"\n{Colors.WARNING}You: {Colors.ENDC}")
        except EOFError:
            break

        if user_input.lower() in ['exit', 'quit', 'bye']:
            print_slow(f"\n{Colors.BLUE}Lumina: Goodbye, {user_name}! Have a wonderful day. ✨{Colors.ENDC}")
            break

        response = get_lumina_response(user_input, context)
        print(f"{Colors.BLUE}Lumina: {response}{Colors.ENDC}")

if __name__ == "__main__":
    main()
