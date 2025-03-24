import time

def ask_question(question):
    print(question)
    start_time = time.time()
    answer = input("你的回答是（请按回车确认）: ").strip()
    end_time = time.time()
    return answer, end_time - start_time

def analyze_answers(control_answers, relevant_answers):
    control_times = [time_taken for _, time_taken in control_answers]
    relevant_times = [time_taken for _, time_taken in relevant_answers]
    
    # 假设回答时间越长，越可能说谎（这是一个非常简化的假设）
    average_control_time = sum(control_times) / len(control_times)
    average_relevant_time = sum(relevant_times) / len(relevant_times)
    
    # 简单的判断逻辑：如果相关问题平均回答时间超过控制问题平均时间的两倍，则标记为可能说谎
    if average_relevant_time > 2 * average_control_time:
        return "可能说谎"
    else:
        return "诚实"

def main():
    # 控制问题
    control_questions = [
        "你的生日是什么时候？",
        "你最喜欢的颜色是什么？",
        "你的家乡在哪里？"
    ]
    
    # 相关问题（假设我们要测试的是某次考试是否作弊）
    relevant_questions = [
        "你在最近的数学考试中是否作弊了？",
        "你是否知道考试中的某些答案是通过不正当手段获得的？"
    ]
    
    control_answers = [ask_question(q) for q in control_questions]
    relevant_answers = [ask_question(q) for q in relevant_questions]
    
    # 打印用户回答（仅用于调试或记录）
    print("\n你的回答：")
    for q, a, t in zip(control_questions, [a for a, t in control_answers], control_times):
        print(f"{q} -> 回答: {a}, 时间: {t:.2f}秒")
    
    for q, a, t in zip(relevant_questions, [a for a, t in relevant_answers], relevant_times):
        print(f"{q} -> 回答: {a}, 时间: {t:.2f}秒")
    
    # 分析回答
    result = analyze_answers(control_answers, relevant_answers)
    print(f"\n测谎结果: {result}")

if __name__ == "__main__":
    main()
