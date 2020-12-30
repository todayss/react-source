// ReactDOM 调用render方法渲染组件
ReactDom.render(element, container, callback)

function render(element, container, callback) {
  // 调用renderSubTreeIntoContainer方法
  renderSubTreeIntoContainer(null, element, container, false, callback)
}

function renderSubTreeIntoContainer(parentComponent, children, container, forceHydrate, callback) {
  // 判断parentComponent是否有值
  // 创建fiberRoot, 然后开始渲染
  let root:Root = container._reactRootContainer // undefined
  if(!root) {
    root = container._reactRootContainer = legacyCreateRootFromDOMContainer(container, forceHydrate)
    // 开始渲染
    root.render(children, callback)
  }
}


// 创建fiberRoot
function legacyCreateRootFromDOMContainer(container, forceHydrate) {
  // Legacy roots are not async by default.
  const isConcurrent = false;
  return new ReactRoot(container, isConcurrent, shouldHydrate);
}


function ReactRoot(container, isConcurrent, shouldHydrate) {
  // root: 创建一个fiberRoot
  const root =  DOMRenderer.createContainer(container, isConcurrent, shouldHydrate)
  this._internalRoot = root
}

function createContainer(
  containerInfo: Container,
  isConcurrent: boolean,
  hydrate: boolean,
): OpaqueRoot {
  return createFiberRoot(containerInfo, isConcurrent, hydrate);
}

// 创建fiberRoot
function createFiberRoot(
  containerInfo: any,
  isConcurrent: boolean,
  hydrate: boolean,
): FiberRoot {
  // Cyclic construction. This cheats the type system right now because
  // stateNode is any.
  const uninitializedFiber = createHostRootFiber(isConcurrent);

  let root;
  if (enableSchedulerTracing) {
    root = ({
      current: uninitializedFiber,
      containerInfo: containerInfo,
      pendingChildren: null,

      earliestPendingTime: NoWork,
      latestPendingTime: NoWork,
      earliestSuspendedTime: NoWork,
      latestSuspendedTime: NoWork,
      latestPingedTime: NoWork,

      didError: false,

      pendingCommitExpirationTime: NoWork,
      finishedWork: null,
      timeoutHandle: noTimeout,
      context: null,
      pendingContext: null,
      hydrate,
      nextExpirationTimeToWorkOn: NoWork,
      expirationTime: NoWork,
      firstBatch: null,
      nextScheduledRoot: null,

      interactionThreadID: unstable_getThreadID(),
      memoizedInteractions: new Set(),
      pendingInteractionMap: new Map(),
    }: FiberRoot);
  } else {
    root = ({
      current: uninitializedFiber,
      containerInfo: containerInfo,
      pendingChildren: null,

      earliestPendingTime: NoWork,
      latestPendingTime: NoWork,
      earliestSuspendedTime: NoWork,
      latestSuspendedTime: NoWork,
      latestPingedTime: NoWork,

      didError: false,

      pendingCommitExpirationTime: NoWork,
      finishedWork: null,
      timeoutHandle: noTimeout,
      context: null,
      pendingContext: null,
      hydrate,
      nextExpirationTimeToWorkOn: NoWork,
      expirationTime: NoWork,
      firstBatch: null,
      nextScheduledRoot: null,
    }: BaseFiberRootProperties);
  }

  uninitializedFiber.stateNode = root;

  // The reason for the way the Flow types are structured in this file,
  // Is to avoid needing :any casts everywhere interaction tracing fields are used.
  // Unfortunately that requires an :any cast for non-interaction tracing capable builds.
  // $FlowFixMe Remove this :any cast and replace it with something better.
  return ((root: any): FiberRoot);
}


// 开始渲染
function render() {
  updateContainer()
}

function updateContainer() {
  scheduleRootUpdate()
}

function scheduleRootUpdate () {
 const update = createUpdate()
  enqueueUdate(current, update)
  // 开始任务调度
  scheduleWork(current, expirationTime)
}
