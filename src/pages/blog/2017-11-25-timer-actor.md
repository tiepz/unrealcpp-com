---
templateKey: blog-post
path: /timer-actor
title: Timer Actor
author: Harrison McGuire
authorImage: 'https://avatars1.githubusercontent.com/u/5263612?s=460&v=4'
authorTwitter: HarryMcGueeze
featuredImage: >-
  https://res.cloudinary.com/several-levels/image/upload/q_80/v1511556058/landscape-purple_bcgzbg.jpg
featuredVideo: youtube.com
tags:
  - beginner
  - timer
uev: 4.18.1
date: 2017-11-25T13:22:13.628Z
description: How to set a log message to print to screen every 2 seconds
---
We'll create a new actor called `TimerActor`. 

In the header file we will add a function to repeat every 2 seconds a `FTimerHandle` class to manage the function in the world's time.

### TimerActor.h
```cpp
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "TimerActor.generated.h"

UCLASS()
class UNREALCPP_API ATimerActor : public AActor
{
	GENERATED_BODY()
	
public:	
	// Sets default values for this actor's properties
	ATimerActor();

protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

	void RepeatingFunction();
	
	FTimerHandle MemberTimerHandle;
	
};
```

In the `.cpp` file, it is necessary to include the `TimerManager.h` file. The `TimerManager.h` is necessary if we want use the engines World Time Manager. You add `TimerManager.h` by adding `#include "TimManager.h"` below the actor's header file.

On `BeginPlay()` set our world timer to play our `RepeatingFunction()` every 2 seconds after 5 seconds of play. So when you push play, wait 5 seconds and the function will then play every 2 seconds. The repeating function is very simple function that prints to the screen.

### TimerActor.cpp
```cpp
#include "TimerActor.h"
#include "TimerManager.h"


// Sets default values
ATimerActor::ATimerActor()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;	

}

// Called when the game starts or when spawned
void ATimerActor::BeginPlay()
{
	Super::BeginPlay();

	// connect timer function to actor. After 5 seconds run RepeatingFunction every 2 seconds 
	GetWorldTimerManager().SetTimer(MemberTimerHandle, this, &ATimerActor::RepeatingFunction, 2.0f, true, 5.0f);
}

// Called every frame
void ATimerActor::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

}

void ATimerActor::RepeatingFunction()
{
	GEngine->AddOnScreenDebugMessage(-1, 5.f, FColor::Red, TEXT("Hello Timer"));
}
```