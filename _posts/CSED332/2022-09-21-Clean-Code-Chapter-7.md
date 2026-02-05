---
layout: post
title: "[Clean Code] Chapter 7. Error Handling"
date: 20220921
tag: [Book, SD]
published: false
---
<!-- *이 문서는 2022년도 가을학기 포스텍 박성우 교수님의 소프트웨어설계 수업을 들으며 Clean Code를 요약한 문서입니다. `Thoughts` 항목은 개인적인 생각이며, 본문 또한 직접적인 번역이 아닌 주관이 들어간 각색임을 유의해 주시기 바랍니다. 오타나 지적은 항상 환영합니다.* 😄

<hr> -->

적절한 error handling을 통해 더 우아하고 탄탄한 코드를 구성할 수 있다.

## Use Exceptions Rather Than Return Codes
보통 각각의 조건을 검사하고 error handling을 수행하는 방식으로 코드를 구성하는데, 이렇게 구성하였을 때의 한계는 logic이 지나치게 길어지거나 까먹을 수 있다는 점이다. 다음 두 예시를 살펴보자.

```Java
public class DeviceController {
    ...
    public void sendShutDown() {
        DeviceHandle handle = geHandle(DEV1);
        // Check the state of the device
        if (handle != DeviceHandle.INVALID) {
            // Save the device status to the record field
            retrieveDeviceRecord(handle);
            // If not suspended, shut down
            if (record.getStatus() != DEVICE_SUSPENDED) {
                pauseDevice(handle);
                clearDeviceWorkQueue(handle);
                closeDevice(handle);
            } else {
                logger.log("Device suspended. Unable to shut down")l
            }
        } else {
            logger.log("Invalid handle for: " + DEV1.toString());
        }
    }
}
```

```Java
public class DeviceController {
    ...
    public void sendShutDown() {
        try {
            tryToShutDown();
        } catch (DeviceShutDownError e) {
            logger.log(e);
        }
    }

    private void tryToShutDown() throws DeviceShutDownError {
        DeviceHandle handle = getHandle(DEV1);
        DeviceRecord record = retrieveDeviceRecord(handle);

        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);
    }

    private DeviceHandle getHandle (DeviceID id) {
        ...
        throw new DeviceShutDownError("Invalid handle for: " + id.toString());
        ...
    }
    ...
}
```

## Write Your `Try-Catch-Finally` Statement First
Exception을 처리함에 앞서 `try-catch-fianlly`문을 기본으로 코드를 구성하면 좋다. 특히 `catch`문에서 특정한 type의 error를 받도록 처리하면 더 직관적인 코드를 작성할 수 있다.

## Provide Context with Exceptions
Error message를 작성할 때 충분한 정보를 담은 message를 작성하여 어떤 맥락에서 발생시키는 error인지를 알 수 있도록 한다.

## Define the Normal Flow
적절한 위치에 error handling logic을 배치하여 의도하지 않은 상황을 예방하는 것은 좋다. 하지만, 다음과 같은 경우는 어떨까.

```Java
try {
    MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
    m_total += expenses.getTotal();
} catch(MealExpensesNotFound e) {
    m_total += getMealPerDiem();
}
```

`catch`문으로 error 상황을 받아 다른 연산을 수행한다는 것이 이상하게 느껴질 것이다. 위의 코드는 다음과 같이 수정할 수 있다.

```Java
MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();
```
여기서 더 간결하고 깔끔한 코드를 작성하는 방법은 없을까? **SPECIAL CASE PATTERN**이라고 부르는 class를 통해 해결할 수 있다.

```Java
public class PerDiemMealExpenses implements MealExpenses {
    public int getTotal() {
        // return the per diem default
    }
}
```

## Don't Return Null
코드를 구성함에 있어 `Null`을 return하지 않도록 짜야 한다. `Null`을 return하는 함수나 method가 있다면, 해당 함수/method를 호출하는 모든 곳에서 null check을 해줘야 하기 때문에 `Null` 사용을 최소화 해야한다. 다음 예시를 살펴보면 어떻게 `Null` 사용을 최소화할 수 있는지 감이 올 것이다.

```Java
List<Employee> employees = getEmployees();
if (employees != null) {
    for(Employee e : employees) {
        totalPay += e.getPay();
    }
}
```

```Java
public List<Employee> getEmployees() {
    if (no employees) {
        return Collections.emptyList();
    }
} 

List<Employee> employees = getEmployees();
for (Employee e : employees) {
    totalPay += e.getPay();
}
```

## Don't Pass Null
`Null`을 return하는 것은 안좋지만, `Null`값을 passing하는 것은 더 안좋다. 함수의 인자나 method의 결과로 `Null` 값을 넘겨주면, 해당 값을 사용할 때 null check을 수행해야 한다. 적절한 error handling을 통해 run time error를 막을 수는 있지만, 애초에 `Null`을 넘겨주지 않는다면 error handling을 신경쓰지 않아도 되고 코드 또한 간결해진다.

## Thoughts
`try-catch-finally`문을 이용하여 error handling을 수행한다는 것은 알고 있었지만, 사실 지금까지 코딩을 하면서 error handling을 신경써본 경험이 잘 없어 이전 Chapter들에 비해 덜 감명깊었던 것 같다. 거의 backend 코드를 짤 때에만 `try-catch`문을 사용해봤던 것 같은데, 이렇게 logging이나 중단되지 않고 계속 실행되는 것이 중요한 production code를 작업할 때에는 필수적으로 가능한 error들에 대한 handling을 꼼꼼하게 수행해야겠다는 생각이 들었다.

<hr>

## Reference
- Clean Code
