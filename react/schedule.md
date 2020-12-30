Scheduler流程概览

function scheduleWork(fiber, expirationTime) {
  const root = scheduleWorkToRoot(fiber, expirationTime)
}

function scheduleWorkToRoot(fiber, expirationTime):FiberRoot | null {
  recordScheduleUpdate()
}
