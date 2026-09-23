# AssetRipper (OdinInspector fix fork)

This is a narrow, one-purpose fork. It's for ripping games that use OdinInspector heavily, nothing else.

For some reason, OdinInspector slaps `[SerializeReference]` on fields that shouldn't have it (in a normal ripping sense). mainly plain `UnityEngine.Object` derived arrays like `AudioSource[]`. But Unity doesn't support that as a polymorphic managed reference, so its serializer just writes the field as an ordinary PPtr array. 

Stock AssetRipper does not expect that that: it assumed any `[SerializeReference]` field always has real managed-reference registry data behind it, threw a `NotSupportedException` on the resulting format tag, and as a side effect dropped every single field on the containing MonoBehaviour, not just the one that confused it. 

For gorilla tag specifically: 
`VRRig.cs` was the main one that kept coming out completely empty because of this.

My fix lives in `FieldSerializer.IsUnityEngineObjectSerializeReference` and a small fallback in `ManagedReferenceRegistryReader` it catches that specific misuse and reads the field as a normal PPtr instead of routing it through the managed reference registry.

If you're not ripping a game that HEAVILY uses OdinInspector, you dont need this fork. Use upstream AssetRipper.
