# AI-Email-Reply-Agent-CrewAI-LangChain-Colab-
https://colab.research.google.com/drive/1y_PGcgru6oK2l4RvFnP_2j6zqP0V71Hh#scrollTo=VDbaOCRVk1XN
# 1. Install dependencies
!pip install -q langchain langchain-openai python-dotenv

# 2. Import libraries
import os
from langchain_openai import ChatOpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

# 3. Set OpenAI API Key
# ⚠️ Yahan apna OpenAI API key dalo. Free key: https://platform.openai.com/api-keys


# 4. Initialize LLM
# gpt-3.5-turbo sasta + fast hai. Agar nahi hai to gpt-4o-mini use karo
llm = ChatOpenAI(
    model="gpt-3.5-turbo",
    temperature=0.3  # Low temp = professional + consistent replies
)
print("✅ LLM Initialized Successfully")

# 5. Build the Agent using Prompt Template
email_prompt = PromptTemplate(
    input_variables=["email_text"],
    template="""
You are a professional Email Reply Assistant. 
Your job is to read the email below and write a polite, clear, and context-aware reply.

Rules:
1. Start with a proper greeting.
2. Address the main point/ask from the original email.
3. Keep tone professional and courteous.
4. End with proper closing + signature placeholder.

Original Email:
{email_text}

Now write the reply email:
"""
)

# Create the chain = Agent
reply_agent = LLMChain(llm=llm, prompt=email_prompt)
print("✅ Email Reply Agent Built Successfully")

# Function to generate a polite email reply

def generate_email_reply(email_text):
    
    email_text = email_text.lower()

    if "meeting" in email_text:
        return """
Dear Sender,

Thank you for your email.

I appreciate the meeting invitation. I am available and would be happy to attend. Please let me know if there are any specific materials or preparations required beforehand.

Looking forward to meeting with you.

Best regards
"""

    elif "assignment" in email_text:
        return """
Dear Sender,

Thank you for reaching out.

I appreciate your message regarding the assignment. I will review the details carefully and ensure that the required work is completed as soon as possible.

Please let me know if there is any additional information that I should consider.

Best regards
"""

    else:
        return """
Dear Sender,

Thank you for your email.

I appreciate you taking the time to contact me. I have received your message and will review it carefully. I will get back to you shortly with any necessary information or updates.

Best regards
"""sample_email = """
Hello,

I would like to schedule a meeting next week to discuss the project progress.

Thank you.
"""

reply = generate_email_reply(sample_email)

print("Original Email:")
print(sample_email)

print("\nGenerated Reply:")
print(reply)sample_email = """
Subject: Meeting Reschedule Request

Hi Team,

I hope you are doing well. I have a conflict tomorrow at 2 PM and won't be able to join our weekly sync. 
Can we please reschedule it to Thursday 11 AM? Let me know if that works for everyone.

Thanks,
Eman
"""

print("\n" + "="*50)
print("INPUT EMAIL:")
print("="*50)
print(sample_email)

